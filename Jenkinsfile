pipeline {
    agent {
        label 'master'
    }
    options {
        timestamps()
    }
    stages{
        stage('push image to nexus registry') 
        {
            steps 
            {
                sh '''
                docker login 127.0.0.1.nip.io -u admin -p admin
                docker tag nodejs_paz:1 127.0.0.1.nip.io/nodejs_ritter:${TAG_NAME}
                docker push 127.0.0.1.nip.io/nodejs_ritter:${TAG_NAME}
                '''
            }
        }
        stage('pull image from registry and run the pod')
        {
            steps
            {
                sh'''
                export KUBECONFIG="/home/jenkins/.kube/config"
                sed -i -e "s/TAG_NAME/${TAG_NAME}/g" my-app-deployment.yaml
                kubectl apply -f my-app-deployment.yaml
                '''
                script
                {
                    def counter = 0
                    while (counter <= 10)
                    {
                        def is_running = sh(script: "kubectl get pods -A | grep my-app |grep Running", returnStatus:true)
                        if (is_running == 0)
                        {
                            break
                        }
                        counter = counter + 1
                    }
                }
                sh '''
                    if [[ ${counter} -eq 10 ]]; then
                        echo "pod my-app failed to start"
                        exit 1
                    fi
                kubectl get pods -A | grep my-app |grep Running
                kubectl port-forward service/my-app 3001:3001
                '''
            }
        }
    }
}

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
                def counter=0
                while [ $counter -le 10 ]
                do
                    echo "Welcome $counter times"
                    def is_running=`kubectl get pods -A | grep my-app |grep Running`
                    if [[ $is_running -eq 0 ]]; then
                        break
                    fi
                    counter=$(( $counter + 1 ))

                done
                if [[ $counter -eq 10 ]]; then
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

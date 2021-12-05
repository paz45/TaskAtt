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
                withCredentials([file(credentialsId: '127.0.0.1', variable: 'KUBECRED')]) 
                {
                    sh'''
                    sudo export $KUBECRED > "/home/paz/.minikube/profiles/minikube/client.key"
                    sudo export KUBECONFIG="/home/paz/.minikube/profiles/minikube/client.key"
                    sed -i -e "s/TAG_NAME/${TAG_NAME}/g" my-app-deployment.yaml
                    kubectl apply -f my-app-deployment.yaml
                    kubectl port-forward service/my-app 3001:3001
                    '''
                }
            }
        }
    }
}

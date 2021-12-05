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
                sed -i -e "s/TAG_NAME/${TAG_NAME}/g" my-app-deployment.yaml
                kubectl apply -f my-app-deployment.yaml
                kubectl port-forward service/my-app 3001:3001
                '''
            }
        }
    }
}

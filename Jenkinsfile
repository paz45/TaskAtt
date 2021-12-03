pipeline {
    agent {
        label 'master'
    }
    options {
        timestamps()
        // gitLabConnection('SQream_gitlab')
    }
    stages{
        stage('install minikube') {
            steps 
            {
                sh'''
                    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
                    sudo install minikube-linux-amd64 /usr/local/bin/minikube
                    minikube start
                '''
            }
        }
    }
}
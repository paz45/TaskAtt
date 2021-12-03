pipeline {
    agent {
        label 'master'
    }
    options {
        timestamps()
        // gitLabConnection('SQream_gitlab')
    }
    stages{
        stage('install_minikube') {
            steps 
            {
                sh'''
                    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
                    sudo usermod -aG docker $USER && newgrp docker
                    sudo install minikube-linux-amd64 /usr/local/bin/minikube
                    minikube start
                '''
            }
        }
    }
}

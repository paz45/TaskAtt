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
                sh 'curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64'
                sh 'sudo usermod -aG docker $USER && newgrp docker'
                script
                {
                    def installation = sh(script: "sudo install minikube-linux-amd64 /usr/local/bin/minikube", returnStatus:true).trim() as Integer
                    if ($installation != '0')
                    {
                        sh'''
                        docker system prune
                        minikube delete
                        '''
                    }
                }
                sh 'minikube start --driver=docker'
            }
        }
    }
}

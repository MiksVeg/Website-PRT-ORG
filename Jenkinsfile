pipeline {
    agent { label 'k8' }

    stages {
        stage('git clone') {
            steps {
                git branch: 'main', url: 'https://github.com/MiksVeg/Website-PRT-ORG.git'
            }
        }
        stage('docker build') {
            steps {
                
                sh "docker build -t miksveg/prt:latest ."
            }
        }
        stage('docker push') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push miksveg/prt:latest"
                    }
                }
            }
        }
        stage('deply on k8') {
            steps {
                sh "kubectl delete -f k8.yaml"
                sh "kubectl create -f k8.yaml"
                sh "kubectl get svc"
            }
        }
    }
}

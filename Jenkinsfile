pipeline {
    agent any
    stages {
        stage('Git Checkout') {
            steps {
                git 'https://github.com/yashighokar1412/nodejs.git'
            }
        }

        stage('Docker Image') {
            steps {
                withDockerRegistry(credentialsId: 'docker', url: 'https://index.docker.io/v1/') {
                    sh "docker build -t yashthedocker/nodejs ."
                    sh "docker push yashthedocker/nodejs:latest"
                    sh "docker images"
                }
            }
        }
    
        stage('Deploy to Kubernetes') {
            steps {
                withAWS(credentials: 'aws') {
                    sh "kubectl apply -f deployment.yaml"
                }
            }
        }
    }
}

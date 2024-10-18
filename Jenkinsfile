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
                withAWS(credentials: 'aws', endpointUrl: 'https://711A4DFE055C0284F9DA129EDB9E3386.gr7.us-east-1.eks.amazonaws.com', region: 'us-east-1') {
                    sh "kubectl apply -f deployment.yaml"
                }
            }
        }
    }
}

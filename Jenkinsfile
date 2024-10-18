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
                    sh "docker build -t yashthedocker/nodejs:01 ."
                    sh "docker push yashthedocker/nodejs:01"
                    sh "docker images"
                }
            }
        }
    
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withAWS(credentials: 'aws', region: 'us-east-1') {
                        withKubeConfig(caCertificate: '', 
                                       clusterName: 'nodejs', 
                                       contextName: 'arn:aws:eks:us-east-1:084375558659:cluster/nodejs', 
                                       credentialsId: 'k8s', 
                                       namespace: 'default', 
                                       restrictKubeConfigAccess: false, 
                                       serverUrl: 'https://711A4DFE055C0284F9DA129EDB9E3386.gr7.us-east-1.eks.amazonaws.com') {
                            sh "kubectl apply -f deployment.yaml"
                            sh "kubectl apply -f service.yml"
                            sh "kubectl get service"
                        }
                    }
                }
            }
        }
    }
}

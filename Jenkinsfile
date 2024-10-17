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

        stage('Build and Zip') {
            steps {
                sh 'zip -r build.zip .'
            }
        }
    
        stage('AWS S3 Upload') {
            steps {
                withAWS(credentials: 'aws', region: 'us-east-1') {
                    s3Upload(bucket: 'my-nodejs-yash', file: 'build.zip', path: 'deployments/build.zip')
                }
            }
        }
    
        stage('Deploy to Kubernetes') {
            steps {
                withAWS(credentials: 'aws', region: 'us-east-1') {
                withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                    sh "kubectl apply -f deployment.yaml"
                }
            }
        }
    }
}

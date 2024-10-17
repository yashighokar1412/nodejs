pipeline {
    agent any
    environment {
        SONAR_HOME = tool "sonar"
    }

    stages {
        stage('Git Checkout') {
            steps {
                git 'https://github.com/yashighokar1412/nodejs.git'
            }
        }

        stage('Sonar Quality Check') {
            steps {
                withSonarQubeEnv(credentialsId: 'sonar', installationName: 'sonar') {
                    sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=nodejs -Dsonar.projectKey=nodejs"
                }
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

        stage('AWS S3 Upload') {
            steps {
                withAWS(credentials: 'aws') {
                    s3Upload(bucket: 'my-bucket', file: 'build.zip', path: 'deployments/build.zip')
                }
            }
        }
    
        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: 'my-cluster', contextName: 'my-context', credentialsId: 'k8s', namespace: 'default', serverUrl: 'https://<cluster-server-url>') {
                    sh "kubectl apply -f deployment.yaml"
                }
            }
        }
    }
}

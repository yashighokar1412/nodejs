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
        stage('deploy-with-ArgoCD') {
            steps {
                withAWS(credentials: 'aws')
                withKubeConfig(caCertificate: '',
                               clusterName: 'node',
                               contextName: 'arn:aws:eks:us-east-1:084375558659:cluster/node',
                               credentialsId: 'k8s',
                               namespace: 'default',
                               restrictKubeConfigAccess: false,
                               serverUrl: 'https://300F08D3D21CEBDE9C9AC1A43A181A80.gr7.us-east-1.eks.amazonaws.com')
                    sh 'kubectl apply -f application.yaml'
                }
            }
        }
    }

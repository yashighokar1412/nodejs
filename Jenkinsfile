pipeline {
    agent any
    envrionment{
        SONAR_HOME= tool "sonar"
    }

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
        stage('k8s deploy') {
            steps {
                withAWS(credentials: 'AWS')
                withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                    sh "kubectl apply -f deployment.yml"
                    sh "kubectl apply -f service.yml"
                }
            }
        }
    }
}

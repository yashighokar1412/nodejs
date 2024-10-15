pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
              git 'https://github.com/yashighokar1412/nodejs.git'            }
        }
    }
       stage('docker image') {
            steps {
              withDockerRegistry(credentialsId: 'docker', url: 'https://index.docker.io/v1/)') }
              }
}

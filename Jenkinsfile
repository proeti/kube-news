pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("proeti/kubenews:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }
        stage('Push image to dockerhub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub')
                    dockerapp.push('latest')
                    dockerapp.push("${env.BUILD_ID}")
                }
            }
        }
    }
}
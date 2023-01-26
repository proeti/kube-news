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
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerehub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
        stage('Deploy app to kubernetes') {
            steps {
                withKubeConfig ([credentialsId: 'kubeconfig']) {
                    sh 'kubectl apply -f ./deployment-kubenews.yaml'
                }
            }
        }
    }
}
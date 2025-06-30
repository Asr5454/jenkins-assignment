@Library('jenkins-shared-lib') _

pipeline {
    agent any

    environment {
        IMAGE_NAME = "asr5454/docker-hello:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/asr5454/docker-hello.git'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        dockerUtils.buildAndPushDockerImage(IMAGE_NAME)
                    }
                }
            }
        }

        stage('Run Container and Health Check') {
            steps {
                sh """
                    docker run -d --name docker-hello ${IMAGE_NAME}
                    sleep 2
                    docker logs docker-hello
                """
            }
        }
    }

    post {
        always {
            sh """
                docker rm -f docker-hello || true
                docker rmi ${IMAGE_NAME} || true
            """
        }
    }
}


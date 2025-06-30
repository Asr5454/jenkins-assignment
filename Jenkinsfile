@Library('jenkins-shared-lib') _

pipeline {
    agent any

    environment {
        IMAGE_NAME = "asr5454/docker-hello:latest"  // Replace with your actual Docker Hub image name if different
        DOCKER_USERNAME = credentials('dockerhub').username
        DOCKER_PASSWORD = credentials('dockerhub').password
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/asr5454/docker-hello.git'  // Replace with your actual repo if different
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    dockerUtils.buildAndPushDockerImage(IMAGE_NAME)
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


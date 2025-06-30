@Library('jenkins-shared-lib') _

pipeline {
    agent any

    environment {
        IMAGE_NAME = "asr5454/http-echo:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/hashicorp/http-echo.git'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                // 👇 Securely load Docker credentials
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
                    docker run -d --name http-echo -p 5678:5678 ${IMAGE_NAME}
                    sleep 5
                    curl http://localhost:5678
                """
            }
        }
    }

    post {
        always {
            sh """
                docker rm -f http-echo || true
                docker rmi ${IMAGE_NAME} || true
            """
        }
    }
}


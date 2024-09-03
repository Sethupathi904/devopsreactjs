pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'Password@9' // Jenkins credentials ID for Docker Hub
        IMAGE_NAME = 'sethu904/react-app' // Docker image name
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm // Checks out code from the repository
            }
        }

        stage('Verify Docker') {
            steps {
                script {
                    sh 'docker --version'
                    sh 'docker info'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${IMAGE_NAME}:${env.BUILD_ID}").push('latest')
                        docker.image("${IMAGE_NAME}:${env.BUILD_ID}").push("${env.BUILD_ID}")
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs() // Cleans workspace after build
        }
    }
}

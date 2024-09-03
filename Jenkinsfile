pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/Sethupathi904/devopsreactjs.git'
        BRANCH = 'main'
        DOCKERHUB_CREDENTIALS_ID = 'dockerhub' // Replace with your Docker Hub credentials ID in Jenkins
        DOCKERHUB_REPO = 'sethu904/devopsreactjs'
        IMAGE_TAG = "${env.BUILD_ID}"
        NODE_VERSION = '16' // Specify the Node.js version you want to use
    }

    stages {
        stage('Start') {
            steps {
                echo 'Starting the Jenkins Pipeline'
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
        }

        stage('Install Node.js') {
            steps {
                script {
                    // Install Node.js if it's not already installed
                    sh "nvm install ${NODE_VERSION}"
                    sh "nvm use ${NODE_VERSION}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install --legacy-peer-deps'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKERHUB_REPO}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS_ID}", passwordVariable: 'DOCKERHUB_PASS', usernameVariable: 'DOCKERHUB_USER')]) {
                    sh 'docker login -u ${DOCKERHUB_USER} -p ${DOCKERHUB_PASS}'
                    sh 'docker push ${DOCKERHUB_REPO}:${IMAGE_TAG}'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Deploying to Kubernetes...'
                // Add your deployment steps here if needed
            }
        }
    }

    post {
        always {
            cleanWs() // Clean workspace after build
        }
    }
}

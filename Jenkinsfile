pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/Sethupathi904/devopsreactjs.git'
        DOCKERHUB_CREDENTIALS_ID = 'dockerhub' // Add your Docker Hub credentials in Jenkins
        DOCKERHUB_REPO = 'sethu904/react-repo'
        IMAGE_TAG = "${env.BUILD_ID}"
    }

    stages {
        stage('Start') {
            steps {
                echo 'Starting the Jenkins Pipeline'
            }
        }

        stage('Checkout SCM') {
            steps {
                git url: "${REPO_URL}", branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
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
                    sh 'docker login -u sethu904 -p ${DOCKERHUB_PASS}'
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

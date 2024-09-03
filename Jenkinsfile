pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Sethupathi904/devopsreactjs.git'
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('sethu904/react-demo-app:${env.BUILD_NUMBER}')
                }
            }
        }
        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        docker.image("<<sethu904/react-demo-app>>:${env.BUILD_NUMBER}").push('latest')
                    }
                }
            }
        }
    }
}
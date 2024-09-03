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

	    stage("Push Docker Image") {
		    steps {
			    script {
				    echo "Push Docker Image"
				    withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {
            				sh "docker login -u sethu904 -p ${dockerhub}"
				    }
				        myimage.push("${env.BUILD_ID}")
				    
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

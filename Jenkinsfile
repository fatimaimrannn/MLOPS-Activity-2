pipeline {
    agent any
    
    environment {
        DOCKER_HUB_REPO = 'your_dockerhub_username/your_repo'
        DOCKER_CREDENTIALS_ID = 'your-jenkins-docker-credentials-id'
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                git url: 'https://github.com/your_username/your_repo.git', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image and tag it with the build number
                    docker.build("${DOCKER_HUB_REPO}:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub and push the Docker image
                    docker.withRegistry('', DOCKER_CREDENTIALS_ID) {
                        docker.image("${DOCKER_HUB_REPO}:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
    }
    
    post {
        always {
            // Clean up workspace after build
            cleanWs()
        }
    }
}

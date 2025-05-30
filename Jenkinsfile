pipeline {
    agent any
    environment {
        // Store GitHub and Docker Hub credentials in environment variables
        GITHUB_TOKEN = credentials('github-credentials') // GitHub personal access token
        DOCKER_HUB_CREDENTIALS = credentials('fatimaimran-dockerhub') // Docker Hub credentials ID
    }
    stages {
        stage('Clone Repository') {
            steps {
                // Cloning the GitHub repository using stored credentials
                git url: 'https://github.com/fatimaimrannn/MLOPS-Activity-2.git',
                    credentialsId: 'github-credentials', // Use the correct credential ID
                    branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    powershell '''
                    docker build -t fatimaimran/ml-app:latest .
                    '''
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    // Log in to Docker Hub and push the image
                    powershell '''
                    echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin
                    docker push fatimaimran/ml-app:latest
                    '''
                }
            }
        }
    }
    post {
        always {
            // Clean up Docker images after the build
            script {
                powershell 'docker rmi fatimaimran/ml-app:latest'
            }
        }
    }
}

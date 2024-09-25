pipeline {
    agent any
    environment {
        GITHUB_CREDENTIALS = credentials('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')  // GitHub personal access token
        DOCKER_HUB_CREDENTIALS = credentials('fatimaimran-dockerhub')  // Docker Hub credentials ID
    }
    stages {
        stage('Clone Repository') {
            steps {
                // Cloning the GitHub repository using stored credentials
                git url: 'https://github.com/fatimaimrannn/MLOPS-Activity-2.git',
                    credentialsId: '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',
                    branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                powershell '''
                docker build -t fatimaimran/ml-app:latest .
                '''
            }
        }
        stage('Push Docker Image') {
            steps {
                powershell '''
                echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin
                docker push fatimaimran/ml-app:latest
                '''
            }
        }
    }
    post {
        always {
            // Optional: Clean up Docker images after the build
            powershell 'docker rmi fatimaimran/ml-app:latest'
        }
    }
}

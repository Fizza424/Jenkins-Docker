pipeline {
    agent any
    environment {
        IMAGE_NAME = "fizza424/docker-working"
    }
    stages {
        stage('Build Docker Image') {
            steps {
                bat "docker build -t ${IMAGE_NAME} ."
            }
        }
        stage('Run Docker Container') {
            steps {
                bat "docker run -d -p 5000:5000 --name app ${IMAGE_NAME}"
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat "docker login -u %DOCKER_USER% -p %DOCKER_PASS%"
                    bat "docker tag ${IMAGE_NAME} ${IMAGE_NAME}:latest"
                    bat "docker push ${IMAGE_NAME}:latest"
                }
            }
        }
    }
    post {
        failure {
            echo "❌ Deployment failed!"
        }
        success {
            echo "✅ Deployment succeeded!"
        }
    }
}


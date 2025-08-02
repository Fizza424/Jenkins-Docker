pipeline {
    agent any

    environment {
        IMAGE_NAME = "fizza424/Jenkins-Docker"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                bat "docker build -t %IMAGE_NAME% ."
            }
        }

        stage('Checkout'){
            steps{
                git branch' main' , url: 'https://Fizza424/Jenkins-Docker'
            }
        }

        stage('Run Docker Container') {
            steps {
                bat '''
                    docker stop app || exit 0
                    docker rm app || exit 0
                    docker run -d -p 5000:5000 --name app %IMAGE_NAME%
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat '''
                        docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                        docker push %IMAGE_NAME%
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Successfully deployed!"
        }
        failure {
            echo "❌ Deployment failed!"
        }
    }
}


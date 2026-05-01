pipeline {
    agent any

    environment {
        AWS_REGION = "us-east-1"
        ECR_REGISTRY = "003493200271.dkr.ecr.us-east-1.amazonaws.com"
    }

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/UnpredictablePrashant/StreamingApp.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh '''
                sudo docker build -t auth-service ./backend/authService
                sudo docker build -t streaming-service ./backend/streamingService
                sudo docker build -t admin-service ./backend/adminService
                sudo docker build -t chat-service ./backend/chatService
                sudo docker build -t frontend ./frontend
                '''
            }
        }

        stage('Tag Images') {
            steps {
                sh '''
                sudo docker tag auth-service:latest $ECR_REGISTRY/auth-service:latest
                sudo docker tag streaming-service:latest $ECR_REGISTRY/streaming-service:latest
                sudo docker tag admin-service:latest $ECR_REGISTRY/admin-service:latest
                sudo docker tag chat-service:latest $ECR_REGISTRY/chat-service:latest
                sudo docker tag frontend:latest $ECR_REGISTRY/frontend:latest
                '''
            }
        }

        stage('Push to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                sudo docker login --username AWS --password-stdin $ECR_REGISTRY

                sudo docker push $ECR_REGISTRY/auth-service:latest
                sudo docker push $ECR_REGISTRY/streaming-service:latest
                sudo docker push $ECR_REGISTRY/admin-service:latest
                sudo docker push $ECR_REGISTRY/chat-service:latest
                sudo docker push $ECR_REGISTRY/frontend:latest
                '''
            }
        }
    }
}

pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID = '259183056581'
        AWS_DEFAULT_REGION = 'us-east-1'
    }
    stages {
        stage('Checkout Code') {
            steps {
                echo 'Pulling latest code updates from GitHub Repository...'
            }
        }
        stage('Login to AWS ECR') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-credentials']]) {
                    sh 'aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com'
                }
            }
        }
        stage('Build & Push Hello Service') {
            steps {
                sh 'cd backend/helloService && docker build -t streaming-hello-service .'
                sh 'docker tag streaming-hello-service:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://'
                sh 'docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://'
            }
        }
        stage('Build & Push Profile Service') {
            steps {
                sh 'cd backend/profileService && docker build -t streaming-profile-service .'
                sh 'docker tag streaming-profile-service:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://'
                sh 'docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://'
            }
        }
        stage('Build & Push Frontend') {
            steps {
                sh 'cd frontend && docker build -t streaming-frontend .'
                sh 'docker tag streaming-frontend:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://'
                sh 'docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://'
            }
        }
    }
}

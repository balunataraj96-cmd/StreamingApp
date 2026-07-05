pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID = '259183056581'
        AWS_DEFAULT_REGION = 'us-east-1'
    }
    stages {
        stage('Checkout Code') {
            steps {
                echo 'Pulling latest repository updates from GitHub...'
            }
        }
        stage('Compile and Push All Services') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-text-credentials', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    
                    echo 'Authenticating with AWS ECR using Container Runtime Bridge...'
                    sh "docker run --rm -e AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} -e AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} amazon/aws-cli ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
                    
                    echo 'Building and Pushing Hello Service...'
                    sh 'cd backend/helloService && docker build -t streaming-hello-service .'
                    sh 'docker tag streaming-hello-service:latest ://amazonaws.com'
                    sh 'docker push ://amazonaws.com'
                    
                    echo 'Building and Pushing Profile Service...'
                    sh 'cd backend/profileService && docker build -t streaming-profile-service .'
                    sh 'docker tag streaming-profile-service:latest ://amazonaws.com'
                    sh 'docker push ://amazonaws.com'
                    
                    echo 'Building and Pushing Frontend Application...'
                    sh 'cd frontend && docker build -t streaming-frontend .'
                    sh 'docker tag streaming-frontend:latest ://amazonaws.com'
                    sh 'docker push ://amazonaws.com'
                }
            }
        }
    }
}

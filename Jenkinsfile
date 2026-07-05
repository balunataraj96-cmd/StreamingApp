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
                // 1. We run commands inside the AWS CLI container directly using an on-the-fly execution mapping
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-credentials']]) {
                    
                    echo 'Logging into Amazon ECR Private Vault...'
                    sh "sudo docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -e AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} -e AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} amazon/aws-cli ecr get-login-password --region ${AWS_DEFAULT_REGION} | sudo docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
                    
                    echo 'Building and Pushing Hello Service...'
                    sh "cd backend/helloService && sudo docker build -t streaming-hello-service ."
                    sh "sudo docker tag streaming-hello-service:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://"
                    sh "sudo docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://"
                    
                    echo 'Building and Pushing Profile Service...'
                    sh "cd backend/profileService && sudo docker build -t streaming-profile-service ."
                    sh "sudo docker tag streaming-profile-service:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://"
                    sh "sudo docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://"
                    
                    echo 'Building and Pushing Frontend Application...'
                    sh "cd frontend && sudo docker build -t streaming-frontend ."
                    sh "sudo docker tag streaming-frontend:latest ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://"
                    sh "sudo docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}://"
                }
            }
        }
    }
}

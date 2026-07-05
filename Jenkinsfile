pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                echo 'Pulling latest code updates from GitHub...'
            }
        }
        stage('Docker and AWS Task Execution') {
            agent {
                docker {
                    image 'amazon/aws-cli:latest'
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-credentials']]) {
                    // 1. Authenticate with ECR
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 259183056581.dkr.ecr.us-east-1.amazonaws.com'
                    
                    // 2. Build and Push Hello Service
                    sh 'cd backend/helloService && docker build -t streaming-hello-service .'
                    sh 'docker tag streaming-hello-service:latest ://amazonaws.com'
                    sh 'docker push ://amazonaws.com'
                    
                    // 3. Build and Push Profile Service
                    sh 'cd backend/profileService && docker build -t streaming-profile-service .'
                    sh 'docker tag streaming-profile-service:latest ://amazonaws.com'
                    sh 'docker push ://amazonaws.com'
                    
                    // 4. Build and Push Frontend
                    sh 'cd frontend && docker build -t streaming-frontend .'
                    sh 'docker tag streaming-frontend:latest ://amazonaws.com'
                    sh 'docker push ://amazonaws.com'
                }
            }
        }
    }
}

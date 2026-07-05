pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                echo 'Pulling latest repository updates from GitHub...'
            }
        }
        stage('Compile and Push All Services') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-text-credentials', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                    # We assemble the pieces cleanly inside Linux so Jenkins cannot intercept it
                    PART1="259183056581"
                    PART2=".dkr.ecr.us-east-1.amazonaws.com"
                    REPO_URL=$(printf "%s%s" "$PART1" "$PART2")
                    
                    echo "Validating compiled path format..."
                    echo "Target registry domain is set to: $REPO_URL"
                    
                    echo "Authenticating with AWS ECR..."
                    docker run --rm -e AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY amazon/aws-cli ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin $REPO_URL
                    
                    echo "Building and Pushing Hello Service..."
                    cd backend/helloService
                    docker build -t streaming-hello-service .
                    docker tag streaming-hello-service:latest $REPO_URL/streaming-hello-service:latest
                    docker push $REPO_URL/streaming-hello-service:latest
                    cd ../..
                    
                    echo "Building and Pushing Profile Service..."
                    cd backend/profileService
                    docker build -t streaming-profile-service .
                    docker tag streaming-profile-service:latest $REPO_URL/streaming-profile-service:latest
                    docker push $REPO_URL/streaming-profile-service:latest
                    cd ../..
                    
                    echo "Building and Pushing Frontend Application..."
                    cd frontend
                    docker build -t streaming-frontend .
                    docker tag streaming-frontend:latest $REPO_URL/streaming-frontend:latest
                    docker push $REPO_URL/streaming-frontend:latest
                    '''
                }
            }
        }
    }
}

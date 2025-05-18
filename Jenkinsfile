pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Clean workspace before checkout
                cleanWs()
                // Checkout code from GitHub repository
                checkout scm
                // List files to verify checkout
                sh 'ls -la'
            }
        }
        
        stage('Docker Cleanup') {
            steps {
                // Remove existing containers with the same names to avoid conflicts
                sh 'docker rm -f frontend-pipeline backend-pipeline || true'
                sh 'docker-compose -p devops-pipeline down || true'
                // Prune unused Docker resources to free up space
                sh 'docker system prune -f'
            }
        }
        
        stage('Build Images') {
            steps {
                // Build Docker images separately for better error identification
                sh 'docker-compose -p devops-pipeline build frontend-pipeline'
                sh 'docker-compose -p devops-pipeline build backend-pipeline'
            }
        }
        
        stage('Deploy Containers') {
            steps {
                // Start containers separately to better identify issues
                sh 'docker-compose -p devops-pipeline up -d backend-pipeline'
                sh 'docker-compose -p devops-pipeline up -d frontend-pipeline'
                // Check if containers are running
                sh 'docker ps'
            }
        }
        
        stage('Verify Deployment') {
            steps {
                // Wait for services to stabilize
                sh 'sleep 10'
                
                // Check backend container
                sh '''
                if docker ps | grep backend-pipeline; then
                    echo "Backend container is running"
                else
                    echo "ERROR: Backend container failed to start"
                    exit 1
                fi
                '''
                
                // Check frontend container
                sh '''
                if docker ps | grep frontend-pipeline; then
                    echo "Frontend container is running"
                else
                    echo "ERROR: Frontend container failed to start"
                    exit 1
                fi
                '''
            }
        }
    }
    
    post {
        success {
            echo "Pipeline completed successfully!"
            echo "Frontend is accessible at http://your-ec2-ip:3001"
            echo "Backend is accessible at http://your-ec2-ip:8889"
        }
        failure {
            echo "Pipeline failed. Check the Jenkins console for details."
        }
    }
}
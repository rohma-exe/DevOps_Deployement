pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Clean workspace before checkout
                cleanWs()
                // Checkout code from GitHub repository
                checkout scm
            }
        }
        
        stage('Build and Deploy') {
            steps {
                // Use docker-compose with custom project name to avoid conflicts
                sh 'docker-compose -p devops-pipeline -f docker-compose.yml down || true'
                sh 'docker-compose -p devops-pipeline -f docker-compose.yml build'
                sh 'docker-compose -p devops-pipeline -f docker-compose.yml up -d'
            }
        }
        
        stage('Verify Deployment') {
            steps {
                // Verify that the containers are running
                sh 'docker ps | grep -E "frontend-pipeline|backend-pipeline"'
            }
        }
    }
    
    post {
        failure {
            // Clean up in case of failure
            sh 'docker-compose -p devops-pipeline -f docker-compose.yml down || true'
        }
    }
}

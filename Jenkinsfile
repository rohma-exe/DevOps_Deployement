pipeline {
  agent any

  environment {
    COMPOSE_PROJECT_NAME = "devops_assignment_v2"
    COMPOSE_FILE = "docker-compose.yml"
  }

  stages {
    stage('Clone Repository') {
      steps {
        git branch: 'main', url: 'https://github.com/rohma-exe/DevOps_Deployement.git'
      }
    }

    stage('Clean up Old Containers and Images') {
      steps {
        script {
          sh '''
            echo "Removing existing containers if any..."
            docker rm -f backend-ci || true
            docker rm -f frontend-ci || true

            echo "Removing old images with incorrect tags (if any)..."
            docker rmi devops_deployement-backend || true
            docker rmi devops_assignment_v2-backend || true
          '''
        }
      }
    }

    stage('Build and Start Containers') {
      steps {
        script {
          sh '''
            echo "Rebuilding containers with correct compose project name..."
            docker-compose -p $COMPOSE_PROJECT_NAME -f $COMPOSE_FILE up -d --build
          '''
        }
      }
    }

    stage('Verify Running Containers') {
      steps {
        script {
          sh 'docker ps'
        }
      }
    }
  }

  post {
    always {
      echo 'Pipeline execution completed.'
    }
  }
}

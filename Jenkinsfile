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

    stage('Verify Compose File') {
      steps {
        script {
          echo "Displaying docker-compose.yml contents for verification:"
          sh 'cat docker-compose.yml'
        }
      }
    }

    stage('Build Docker Containers') {
      steps {
        script {
          sh """
            echo "Bringing down existing containers and networks..."
            docker-compose -p $COMPOSE_PROJECT_NAME -f $COMPOSE_FILE down

            echo "Building and starting containers with updated config..."
            docker-compose -p $COMPOSE_PROJECT_NAME -f $COMPOSE_FILE up -d --build

            echo "Listing running containers for verification:"
            docker ps --filter "name=${COMPOSE_PROJECT_NAME}"
          """
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

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

    stage('Build Docker Containers') {
      steps {
        script {
          sh "docker-compose -p $COMPOSE_PROJECT_NAME -f $COMPOSE_FILE up -d --build"
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

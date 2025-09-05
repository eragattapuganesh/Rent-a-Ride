pipeline {
  agent any

  environment {
    // Customize image tags if you want to push to a registry
    FRONTEND_IMAGE = "rentaride/frontend:latest"
    BACKEND_IMAGE  = "rentaride/backend:latest"
    // For docker login (optional): set credentialsId in withCredentials
    // REGISTRY_CREDENTIALS = 'dockerhub-creds'
  }

  options {
    timestamps()
    ansiColor('xterm')
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker images') {
      steps {
        sh '''
          docker build -f backend/Dockerfile -t ${BACKEND_IMAGE} .
          docker build -f client/Dockerfile  -t ${FRONTEND_IMAGE} .
        '''
      }
    }

    // Optional: push to a registry
    // stage('Push images') {
    //   steps {
    //     withCredentials([usernamePassword(credentialsId: REGISTRY_CREDENTIALS, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
    //       sh '''
    //         echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
    //         docker push ${BACKEND_IMAGE}
    //         docker push ${FRONTEND_IMAGE}
    //       '''
    //     }
    //   }
    // }

    stage('Deploy with Docker Compose') {
      steps {
        sh '''
          # Ensure Compose v2 is available as `docker compose`
          docker compose version

          # Bring stack up (rebuild if Dockerfiles changed)
          docker compose -f docker-compose.yml up -d --build

          # Show status
          docker compose ps
        '''
      }
    }
  }

  post {
    always {
      sh 'docker system df || true'
    }
  }
}

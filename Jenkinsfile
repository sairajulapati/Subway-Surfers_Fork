pipeline {
  agent any
  environment {
    IMAGE_NAME = "subway-surfers:${env.BUILD_NUMBER}"
    CONTAINER_NAME = "subway-surfers"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker image') {
      steps {
        sh "docker build -t ${IMAGE_NAME} ."
      }
    }

    stage('Stop & remove old container (if any)') {
      steps {
        sh "docker rm -f ${CONTAINER_NAME} || true"
      }
    }

    stage('Run container') {
      steps {
        // change -p 80:80 if you'd like a different host port
        sh "docker run -d --name ${CONTAINER_NAME} -p 80:80 --restart unless-stopped ${IMAGE_NAME}"
      }
    }
  }

  post {
    success {
      echo "Deployed ${IMAGE_NAME} -> container ${CONTAINER_NAME}"
    }
    always {
      cleanWs()
    }
  }
}

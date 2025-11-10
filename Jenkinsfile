pipeline {
  agent none

  environment {
    IMAGE_NAME = 'hello-node'
    IMAGE_TAG  = 'latest'
    FULL_IMAGE = "${IMAGE_NAME}:${IMAGE_TAG}"
  }

  stages {
    stage('Checkout') {
      agent { label 'any' }
      steps {
        checkout scm
      }
    }

    stage('Docker Build') {
      agent { label 'any' }
      environment {
        DOCKER_HOST = 'tcp://dind:2375'
      }
      steps {
        sh '''
          set -eux
          which docker
          docker version
          docker build . -t ${FULL_IMAGE}
          docker images | head -n 5
        '''
      }
    }
  }

  post {
    success { echo "Built ${env.FULL_IMAGE}" }
    failure { echo 'Build failed.' }
  }
}


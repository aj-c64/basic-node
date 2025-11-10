pipeline {
  agent none

  environment {
    IMAGE_NAME = 'hello-node'
    IMAGE_TAG  = "latest"
    FULL_IMAGE = "${IMAGE_NAME}:${IMAGE_TAG}"
  }

  stages {
    stage('Checkout') {
      agent { label 'node-agent' }
      steps {
        checkout scm
      }
    }

    stage('Docker Build') {
      agent {
        docker {
          image 'docker:27-cli'
          args '--network jenkins-net'
          reuseNode true
        }
      }
      environment {
        DOCKER_HOST = 'tcp://dind:2375'
      }
      steps {
        sh '''
          set -eux
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


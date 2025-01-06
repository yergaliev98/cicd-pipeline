pipeline {
  agent any
  tools {
    nodejs('23.5.0')
  }
  stages {
    stage('Build App') {
      steps {
        sh 'sh \'./scripts/build.sh\''
      }
    }

    stage('Test App') {
      steps {
        sh 'sh \'./scripts/test.sh\''
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'sh \'docker build -t mybuildimage .\'
      }
    }

    stage('Push the image') {
      steps {
        sh '''docker.withRegistry(REGISTRY, DOCKER_CREDS) {
                        // Tag and push the image
                        def image = docker.build("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                        image.push("${env.BUILD_NUMBER}")
                        image.push(\'latest\')
                    }
'''
        }
      }

    }
    environment {
      DOCKER_IMAGE = 'mybuildimage'
      REGISTRY = 'https://registry.hub.docker.com'
      DOCKER_CREDS = 'beka98-dockerhub'
    }
  }

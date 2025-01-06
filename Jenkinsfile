pipeline {
  agent any
  stages {
    stage('Git Checkout') {
      steps {
        git(url: 'https://github.com/yergaliev98/cicd-pipeline', credentialsId: 'jenkins-yergaliev98', branch: 'main')
      }
    }

    stage('Build Application') {
      steps {
        sh 'sh \'./scripts/build.sh\''
      }
    }

    stage('Run Test') {
      steps {
        sh 'sh \'./scripts/test.sh\''
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'sh \'docker build -t mybuildimage\''
      }
    }

  }
  environment {
    DOCKER_IMAGE = 'mybuildimage'
    REGISTRY = 'https://registry.hub.docker.com'
    DOCKER_CREDS = 'beka98-dockerhub'
  }
}
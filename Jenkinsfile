pipeline {
  agent any
  
  environment {
    DOCKER_IMAGE = 'mybuildimage'
    REGISTRY = 'registry.hub.docker.com'  // Using default Docker Hub registry (no need for 'https://')
    DOCKER_CREDS = 'beka98-dockerhub'  // Make sure this credential exists in Jenkins
  }
  
  tools {
    nodejs('23.5.0')  // Ensure this NodeJS tool is configured in Jenkins' Global Tool Configuration
  }

  stages {
    stage('Build App') {
      steps {
        sh 'chmod +x ./scripts/build.sh'
        sh './scripts/build.sh'  // Corrected the syntax for calling the shell script
      }
    }

    stage('Test App') {
      steps {
        sh 'chmod +x ./scripts/test.sh'
        sh './scripts/test.sh'  // Corrected the syntax
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          // Build the Docker image and store it in a variable
          def image = docker.build("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
        }
      }
    }

    stage('Push the image') {
      steps {
        script {
          withDockerRegistry(credentialsId: 'beka98-dockerhub') {
             sh "docker push -t beka98/mybuildimage:${env.BUILD_NUMBER}"
        }
      }
    }
  }
}
}

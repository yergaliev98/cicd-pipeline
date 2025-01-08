pipeline {
  agent any
  
  environment {
    DOCKER_IMAGE = 'mybuildimage'
    REGISTRY = 'registry.hub.docker.com'  // Using default Docker Hub registry (no need for 'https://')
    DOCKER_CREDS = 'beka98-dockerhub'  // Make sure this credential exists in Jenkins
  }
  
  tools {
    nodejs('23.5.0')  // Ensure this NodeJS tool is configured in Jenkins' Global Tool Configuration
    jdk('OpenJDK8')
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


   stage('Push the image') {
      steps {
        script {
         docker.withRegistry(REGISTRY, DOCKER_CREDS) {
                        // Tag and push the image
                        def image = docker.build("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                        image.push("${env.BUILD_NUMBER}")
                        image.push('latest')
                    }
      }
    }
  }
}
}

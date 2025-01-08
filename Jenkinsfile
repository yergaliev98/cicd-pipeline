pipeline {
  agent any
  
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

    stage('Build image') {
        def image = docker.build("beka98/mybuild")
    }

     stage('Push the image') {
          docker.withRegistry('https://registry.hub.docker.com', 'beka98-dockerhub') {
                        // Tag and push the image
                        image.push("${env.BUILD_NUMBER}")
                        image.push('latest')
                    }
      }
    }
  }


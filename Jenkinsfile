pipeline {
    environment {
        imagename = "beka98/mybuild"
        dockerImage = ''
        dockerHubCredentials = 'beka98-dockerhub'
    }
 
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
 
    
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build "${imagename}:latest"
                }
            }
        }
 
 
 
        stage('Deploy Image') {
            steps {
                script {
                    // Use Jenkins credentials for Docker Hub login
                    withCredentials([usernamePassword(credentialsId: dockerHubCredentials, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
 
                        // Push the image
                        sh "docker push ${imagename}:latest"
                    }
                }
            }
        }
    }
}

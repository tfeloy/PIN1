pipeline {
  agent any

  options {
    timeout(time: 2, unit: 'MINUTES')
  }

  environment {
    ARTIFACT_ID = "elbuo8/webapp:${env.BUILD_NUMBER}"
  }

  stages {
    stage('Check Docker Access') {
      steps {
        sh 'docker info'
        sh 'docker ps'
      }
    }

   stage('Login to Docker Registry') {
      steps {
        sh 'docker logout'
        withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
          sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
        }
      }
    }
   stage('Building image') {
      steps{
          sh '''
          docker build -t testapp .
             '''  
        }
    }
  
  
    stage('Run tests') {
      steps {
        sh "docker run testapp npm test"
      }
    }
    stage('Login to Docker Registry') {
      steps {
        sh 'docker logout'
        withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
          sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
        }
      }
    }
   stage('Deploy Image') {
      steps{
        sh '''
        docker tag testapp tfeloy/pip2/testapp
        docker push tfeloy/pip2/testapp
        '''
        }
      }
    }
}


    
  


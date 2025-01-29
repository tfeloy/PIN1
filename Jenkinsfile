pipeline {
  agent any

  options {
    timeout(time: 2, unit: 'MINUTES')
  }

  environment {
    DOCKER_CREDENTIALS = credentials('docker-hub') // ID de tus credenciales
    IMAGE_NAME = "tfeloy/pip2/testapp:${env.BUILD_NUMBER}"
    ARTIFACT_ID = "elbuo8/webapp:${env.BUILD_NUMBER}"
  }

  stages {
   
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
    
   stage('Deploy Image') {
      steps{
        sh "echo \$DOCKER_CREDENTIALS_PSW | docker login -u \$DOCKER_CREDENTIALS_USR --password-stdin"
        sh "docker tag testapp \$IMAGE_NAME"
        sh "docker push \$IMAGE_NAME"
        }
      }
    }
}


    
  


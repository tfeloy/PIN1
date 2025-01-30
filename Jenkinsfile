pipeline {
  agent any

  options {
    timeout(time: 2, unit: 'MINUTES')
  }

  environment {
    DOCKER_CREDENTIALS = credentials('docker-hub')
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
        sh '''
        echo \$DOCKER_CREDENTIALS_PSW | docker login docker.io -u \$DOCKER_CREDENTIALS_USR --password-stdin
        docker tag testapp tfeloy/pip2:testapp
        docker push tfeloy/pip2:testapp   
        '''
        }
      }
    }
}

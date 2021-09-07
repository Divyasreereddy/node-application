pipeline {
  agent any
  stages {
    stage('Maven Build') {
      steps {
        sh 'mvn clean package'
      }

    }
    stage('Building Docker Image') {
      steps {
        sh 'docker build -t divyasreereddy/node-application:0.0.0 .'
      }

    }
     stage('push Docker Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'DockerLogin', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
          sh 'docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}'
           sh 'docker push divyasreereddy/node-application:0.0.0 '
          }
        
      }

    }
  }
}

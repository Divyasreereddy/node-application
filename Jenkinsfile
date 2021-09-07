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
  }
}

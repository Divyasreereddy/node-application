pipeline {
  agent any
  environment {
    PROJECT_VERSION = project_version().trim()

  }
  stages {
    stage('Maven Build') {
      steps {
        sh 'mvn clean package'
      }

    }
    stage('Building Docker Image') {
      steps {
        sh '''
        # project_version=$(cat pom.xml | grep "version" | head -1 | awk '{print $1}' | sed "s/<version>//" | sed "s/<.*//")
        docker build -t divyasreereddy/node-application:0.0.0 . 
        '''
 
      }

    }
     stage('push Docker Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'DockerLogin', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
           sh '''
         #  project_version=$(cat pom.xml | grep "version" | head -1 | awk '{print $1}' | sed "s/<version>//" | sed "s/<.*//")
          docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}
          docker push divyasreereddy/node-application:0.0.0
          '''
          }
        
      }

    }
  }
}
def project_version() {
   return sh(returnStdout: true, script:"cat pom.xml | grep \"version\" | head -1 | awk \'{print \$1}\' | sed \"s/<version>//\" | sed \"s/<.*//\"")
 }

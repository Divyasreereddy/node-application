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
        docker build -t divyasreereddy/node-application:${PROJECT_VERSION} . 
        '''
 
      }

    }
    stage('push Docker Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'DockerLogin', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
           sh '''
         #  project_version=$(cat pom.xml | grep "version" | head -1 | awk '{print $1}' | sed "s/<version>//" | sed "s/<.*//")
          docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}
          docker push divyasreereddy/node-application:${PROJECT_VERSION}
          '''
          }
        }
      }
     stage('Deploy Docker Contioners ') {
      steps {
        sh '''
        ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/docker.pem ec2-user@3.109.3.28 docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}
        # ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/docker.pem ec2-user@3.109.3.28 docker rm -f server1 server2
        ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/docker.pem ec2-user@3.109.3.28 docker pull divyasreereddy/node-application:${PROJECT_VERSION}
        ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/docker.pem ec2-user@3.109.3.28 docker run -d --name server1 -p 8081:8080 divyasreereddy/node-application:${PROJECT_VERSION}
        ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/docker.pem ec2-user@3.109.3.28 docker run -d --name server2 -p 8082:8080 divyasreereddy/node-application:${PROJECT_VERSION}
        '''
      }

    }

    }
  }

def project_version() {
   return sh(returnStdout: true, script:"cat pom.xml | grep \"version\" | head -1 | awk \'{print \$1}\' | sed \"s/<version>//\" | sed \"s/<.*//\"")
 }

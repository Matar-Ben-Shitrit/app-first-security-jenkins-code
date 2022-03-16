pipeline {
  agent {
    dockerfile true
  }
  stages {
    stage('Test App') {
      post {
        always {
          junit 'test-reports/*.xml'
        }

      }
      steps {
        sh 'python3 test.py'
      }
    }

    stage('Build image') {
      steps {
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }

      }
    }

    stage('Upload Image to Registry') {
      steps {
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }

      }
    }

    stage('Remove Unused docker image') {
      steps {
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }

  }
  environment {
    registry = '/my-cicd-app'
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
}
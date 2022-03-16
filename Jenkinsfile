pipeline {
  environment {
    registry = "/matarbsh/my-cicd-app"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent {
    dockerfile true
  }
  stages {
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
          System.setProperty("org.jenkinsci.plugins.durabletask.BourneShellScript.LAUNCH_DIAGNOSTICS", "true");
        }
      }
    }
    stage('Upload Image to Registry') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}

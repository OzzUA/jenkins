pipeline {
  environment {
    registry = "220760/wordpress"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/OzzUA/jenkins.git'
      }
    }
    stage('Building image') {
      steps{
        script {
          GIT_TAG = sh(script: "git tag", returnStdout: true).trim()
          dockerImage = docker.build registry + ":$GIT_TAG"
        }
      }
    }
    stage('Deploy Image') {
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
        sh "docker rmi $registry::$GIT_TAG"
      }
    }
  }
}

pipeline {
  environment {
    registry = "220760/wordpress"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
   parameters {
     gitParameter name: 'TAG',
                  type: 'PT_TAG',
                  defaultValue: 'master'
    }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/OzzUA/jenkins.git'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER" + ":TAG"
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
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}

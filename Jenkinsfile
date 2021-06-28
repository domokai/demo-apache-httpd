pipeline {
  environment {
    registry = "domokai/apache-httpd-beta"
    registryCredential = 'dockerhub_lara'
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'main',
                credentialsId: 'githublara',
                url: 'https://github.com/domokai/demo-apache-httpd.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":v1.0.0"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential  ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Update yaml k8s') {
      steps{
        sh '''
            rm -rf domokai-kubernetes
        '''
      }
    }

  }

}

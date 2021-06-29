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
            git config --global user.name "domokai"
            git config --global user.email "larajorge11@gmail.com"
            git clone --single-branch --branch main git@github.com:domokai/domokai-kubernetes.git
            cd domokai-kubernetes/domokai-dev/apache1
            docker run --rm -v "${PWD}":/workdir mikefarah/yq e '.[0].value = "domokai/apache-httpd-beta:v1.0.0"' --inplace --verbose 'version-patch.yaml'
            cat version-patch.yaml
            git commit -am "updating app: apache1 with version: 1.0.0"
            git remote -v
            git push origin main
            rm -rf domokai-kubernetes
        '''
      }
    }

  }

}

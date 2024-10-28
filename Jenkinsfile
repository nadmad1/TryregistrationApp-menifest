pipeline {
  agent {label 'Jenkins-Agent'}
  environment {
    APP_NAME= "register-app-pipeline"
  }
  stages {
    stage("cleanup workspace") {
      steps {
        deleteDir()
      }
    }
    stage("checkout from scm") {
      steps {
        git branch: 'main', credentialsId: 'GitHub', url:'https://github.com/nadmad1/TryregistrationApp-menifest.git'
      }
    }
    stage("update the Deployment Tags") {
      steps {
        sh """
          cat deployment.yaml
          sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
          cat deployment.yaml
          """
      }
    }
    stage("push the changed deployment file to Git") {
      steps {
        sh """
            git config --global user.name "nadmad1"
            git config --global user.email "nadmad1@gmail.com"
            git add deployment.yaml
            git commit -m "Updated Deploument Manifest"
            """
        withCredentials([gitUsernamePassword(credentialsID: 'GitHub', gitToolName: 'Default')]) {
          sh "git push https://github.com/nadmad1/TryregistrationApp-menifest.git"
        }
      }
    }
  }
}

// This Pipeline is built with Parameter ==> IMAGE_TAG
// This Pipeline is triggered remotely from another pipeline
pipeline {
  agent any
  environment {
    APP_NAME = "registration-app"
  }
  stages {
    stage("Cleanup Workspace") {
      steps {
        cleanWs()
      }
    }
    stage("Checkout from SCM") {
      steps {
        git branch: 'main', 
          url: 'https://github.com/AhmedOkila/registration-app__gitops.git',
          credentialsId: 'Github'
      }
    }
    stage("Update the Deployment Tags") {
      steps {
        sh """
        cat deployment.yaml
        sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
        cat deployment.yaml 
        """
      }
    }
    stage("Push the changed deployment file to Git") {
      steps {
        sh """
        git config --global user.name "Ahmed Okila"
        git config --global user.email "ahmedelsaid3123@gmail.com"
        git add deployment.yaml
        git commit -m "Updating deployment.yaml file"
        """
        withCredentials([gitUsernamePassword(credentialsId: 'Github', gitToolName: 'Default')]) {
          sh "git push https://github.com/AhmedOkila/registration-app__gitops.git main"
        }
      }
    }

  }
}
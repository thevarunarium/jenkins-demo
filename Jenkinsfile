pipeline {
  agent any
  environment {
    APP_NAME = 'jenkins-demo'
    APP_VERSION = '1.0'
  }
  tools {
    maven 'maven-3.9'
  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn clean package'
        archiveArtifacts artifacts: 'target/*.jar'
      }
    }
    stage('Test') {
      steps {
        sh 'mvn test'
      }
    }
    stage('Deploy') {
      steps {
        echo "Deploying ${APP_NAME} version ${APP_VERSION}, build ${env.BUILD_NUMBER}"
      }
    }
  }
  post {
    always  { echo 'This runs every time' }
    success { echo 'Pipeline succeeded' }
    failure { echo 'Pipeline failed' }
  }
}

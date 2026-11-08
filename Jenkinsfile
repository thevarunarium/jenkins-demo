pipeline {
  agent any
  tools {
    maven 'maven-3.9'
  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn -version'
      }
    }
    stage('Test') {
      steps { echo 'Testing...' }
    }
    stage('Deploy') {
      steps { echo 'Deploying...' }
    }
  }
  post {
    always  { echo 'This runs every time' }
    success { echo 'Pipeline succeeded' }
    failure { echo 'Pipeline failed' }
  }
}

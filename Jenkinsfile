pipeline {
  agent any
  stages {
    stage('Build') {
      steps { echo 'Building...' }
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

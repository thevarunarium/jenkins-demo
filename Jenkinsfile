pipeline {
  agent any
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
      steps { sh 'mvn test' }
    }
    stage('Deploy') {
      steps { 
        echo "Deploying build number ${env.BUILD_NUMBER}"
        echo "Job name is ${env.JOB_NAME}"
        echo "Workspace is ${env.WORKSPACE}"
      }
    }
  }
  post {
    always  { echo 'This runs every time' }
    success { echo 'Pipeline succeeded' }
    failure { echo 'Pipeline failed' }
  }
}

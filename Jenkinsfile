
pipeline {
    agent any

    environment {
        APP_NAME = 'jenkins-demo'
        APP_VERSION = '1.0'
        MY_SECRET = credentials('demo-secret')
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
                sh 'echo "Deployment started"'
                sh 'echo "The secret is available as an environment variable"'
            }
        }
    }

    post {
        always {
            echo 'This runs every time'
        }

        success {
            echo 'Pipeline succeeded'
        }

        failure {
            echo 'Pipeline failed'
        }
    }
}

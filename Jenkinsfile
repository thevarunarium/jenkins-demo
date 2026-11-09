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

        stage('Docker Check') {
            steps {
                sh 'docker --version'
                sh 'docker ps'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${APP_NAME}:${env.BUILD_NUMBER} ."
            }
        }

        stage('Deploy') {
            steps {
                sh "docker run --rm ${APP_NAME}:${env.BUILD_NUMBER}"'
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

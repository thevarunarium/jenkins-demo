
pipeline {
    agent any

    parameters {
        booleanParam(
            name: 'RUN_DEPLOY',
            defaultValue: false,
            description: 'Enable or disable Docker deployment'
        )
        choice(
            name: 'ENVIRONMENT',
            choices: ['dev', 'staging', 'prod'],
            description: 'Which environment is this build for?'
        )
        password(
            name: 'API_KEY',
            defaultValue: '',
            description: 'Practice secret, type any fake value'
        )
        
        text(
            name: 'CHANGELOG',
            defaultValue: 'line one\nline two',
            description: 'Multi-line notes for this build'
        )
        string(
            name: 'RELEASE_NOTE',
            defaultValue: 'no note',
            description: 'A short note for this build'
        )
    }

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

        stage('Show Parameters') {
            steps {
                echo "Release note: ${params.RELEASE_NOTE}"
                echo "Environment: ${params.ENVIRONMENT}"
            }
        }
        stage('Prod Warning') {
            when {
                expression { params.ENVIRONMENT == 'prod' }
            }
            steps {
                echo 'WARNING: this build is for PRODUCTION'
            }
        }
        stage('Check Password Param') {
            steps {
                sh 'echo "API key length: ${#API_KEY}"'
            }
        }
        stage('Deploy') {
            when {
                expression {
                    return params.RUN_DEPLOY
                }
            }
            steps {
                sh "docker run --rm ${APP_NAME}:${env.BUILD_NUMBER}"
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

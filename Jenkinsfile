pipeline {
    agent any

    parameters {
        booleanParam(
            name: 'PUSH_IMAGE',
            defaultValue: false,
            description: 'Push the image to Docker Hub'
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

        booleanParam(
            name: 'RUN_DEPLOY',
            defaultValue: false,
            description: 'Run deployment stage'
        )
    }

    environment {
        APP_NAME = 'jenkins-demo'
        APP_VERSION = '1.0'
        MY_SECRET = credentials('demo-secret')
        DOCKERHUB_REPO = 'varunspark/jenkins-demo'
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

        stage('Slow Checks') {
            failFast true

            parallel {
                stage('Check A') {
                    steps {
                        echo 'Check A started'
                        sh 'sleep 10'
                        echo 'Check A finished'
                    }
                }

                stage('Check B') {
                    steps {
                        echo 'Check B started'
                        sh 'sleep 5'
                        echo 'Check B finished'
                    }
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${APP_NAME}:${env.BUILD_NUMBER} ."
            }
        }

        stage('Docker Push') {
            when {
                expression { params.PUSH_IMAGE }
            }
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DH_USER',
                        passwordVariable: 'DH_TOKEN'
                    )
                ]) {
                    sh '''
                        printf '%s' "$DH_TOKEN" | docker login -u "$DH_USER" --password-stdin
                        docker tag ${APP_NAME}:${BUILD_NUMBER} ${DOCKERHUB_REPO}:${BUILD_NUMBER}
                        docker push ${DOCKERHUB_REPO}:${BUILD_NUMBER}
                        docker logout
                    '''
                }
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

        stage('Show Text Param') {
            steps {
                echo "Changelog:\n${params.CHANGELOG}"
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

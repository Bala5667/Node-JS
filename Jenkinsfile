pipeline {
    agent any

    environment {
        IMAGE_NAME = 'bala5667/node-js-app'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Bala5667/Node-JS.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Test') {
            steps {
                bat 'npm test'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat '''
                        echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                        docker push %IMAGE_NAME%
                    '''
                }
            }
        }
    }

    post {
        failure {
            echo '❌ Something went wrong during the pipeline execution.'
        }
        success {
            echo '✅ Pipeline executed successfully!'
        }
    }
}

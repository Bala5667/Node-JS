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

        stage('Docker Build & Test') {
            steps {
                script {
                    // Build Docker image with app and dependencies
                    bat "docker build -t %IMAGE_NAME% ."

                    // Run tests inside the container
                    bat "docker run --rm %IMAGE_NAME% npm test"
                }
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

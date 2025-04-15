pipeline {
    agent {
        docker {
            image 'node:18'
            args '-v $PWD:$PWD'
        }
    }

    environment {
        DOCKER_IMAGE = 'bala123/node-app:V1.0' 
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                echo 'Logging into Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Docker Build & Push') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t $DOCKER_IMAGE .'

                echo 'Pushing Docker image to Docker Hub...'
                sh 'docker push $DOCKER_IMAGE'
            }
        }
    }

    post {
        success {
            echo '✅ Build, test, and Docker push completed successfully!'
        }
        failure {
            echo '❌ Something went wrong during the pipeline execution.'
        }
    }
}

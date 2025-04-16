pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'balaji5667/node-js-app'
        EC2_IP = '100.25.98.236'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'Githubrepo', url: 'https://github.com/Bala5667/Node-JS.git'
            }
        }

        stage('Build & Test') {
            steps {
                script {
                    // Optional: Enable Docker BuildKit for better performance
                    // env.DOCKER_BUILDKIT = 1
                    sh "docker build -t ${DOCKER_IMAGE} ."
                    sh "docker run --rm ${DOCKER_IMAGE} npm test"
                }
            }
        }

        stage('Docker Image') {
            steps {
                sh 'docker images'
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    sh '''
                        ssh -o StrictHostKeyChecking=no -i /var/lib/jenkins/AmazonLinux.pem ubuntu@100.25.98.236 "docker pull balaji5667/node-js-app:latest && docker stop myapp || true && docker rm myapp || true && docker run -d --name myapp -p 80:3000 balaji5667/node-js-app:latest"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}

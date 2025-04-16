pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-creds')
        EC2_SSH_KEY = credentials('ec2-ssh-key')
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Bala5667/Node-JS.git'
            }
        }
        
        stage('Docker Build & Test') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t balaji5667/node-js-app .'
                    
                    // Run the tests inside the Docker container
                    sh 'docker run --rm balaji5667/node-js-app npm test'
                }
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        // Login to Docker Hub using your username
                        sh 'echo $DOCKER_PASS | docker login -u balaji5667 --password-stdin'
                        sh 'docker push balaji5667/node-js-app'
                    }
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    script {
                        // SSH into EC2 instance, pull the Docker image, stop/remove existing container, and run a new container
                        sh '''
                            ssh -o StrictHostKeyChecking=no -i ~/.ssh/id_rsa ubuntu@44.195.20.79 "docker pull balaji5667/node-js-app && \
                            docker stop nodeapp || true && \
                            docker rm nodeapp || true && \
                            docker run -d -p 80:3000 --name nodeapp balaji5667/node-js-app"
                        '''
                    }
                }
            }
        }
    }
    
    post {
        always {
            // Clean up the workspace after the pipeline run
            cleanWs() 
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

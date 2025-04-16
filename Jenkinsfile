pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'balaji5667/node-js-app'
        EC2_IP = '100.25.98.236'
        EC2_USER = 'ubuntu'
        PRIVATE_KEY_PATH = '/home/ubuntu/AmazonLinux.pem'
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
                    sh "docker build -t ${DOCKER_IMAGE} ."
                    sh "docker run --rm ${DOCKER_IMAGE} npm test"
                }
            }
        }

        stage('Docker Image Push') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'balaji5667', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login --username $DOCKER_USERNAME --password-stdin'
                    }
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    sh """
                    ssh -o StrictHostKeyChecking=no -i ${PRIVATE_KEY_PATH} ${EC2_USER}@${EC2_IP} <<EOF
                        docker pull ${DOCKER_IMAGE}
                        docker stop node-js-app || true
                        docker rm node-js-app || true
                        docker run -d --name node-js-app -p 80:80 ${DOCKER_IMAGE}
                    EOF
                    """
                }
            }
        }
    }
}

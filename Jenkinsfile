pipeline { 
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'Githubrepo', url: 'https://github.com/Bala5667/Node-JS.git'
            }
        }

        stage('Build & Test') {
            steps {
                script {
                    sh 'docker build -t balaji5667/node-js-app .'
                    sh 'docker run --rm balaji5667/node-js-app npm test'
                }
            }
        }

        stage('Docker Image') {
            steps {
                script {
                    sh 'docker images'
                }
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'balaji5667', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin docker.io'
                        sh 'docker push balaji5667/node-js-app'
                    }
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ssh-key', keyFileVariable: 'KEY')]) {
                    script {
                        def ec2_ip = '100.25.98.236'

                        sh """
                        chmod 400 "$KEY"
                        ssh -o StrictHostKeyChecking=no -i "$KEY" ubuntu@${ec2_ip} '
                            docker pull balaji5667/node-js-app:latest &&
                            docker stop nodejs-app || true &&
                            docker rm nodejs-app || true &&
                            docker run -d -p 80:3000 --name nodejs-app balaji5667/node-js-app
                        '
                        """
                    }
                }
            }
        }
    }
}

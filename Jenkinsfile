pipeline {
    agent any

    environment {
        IMAGE_NAME = 'balaji5667/node-js-app'
        EC2_HOST = 'ubuntu@44.195.20.79' 
        PEM_FILE = 'C:\\Users\\HP\\Desktop\\Devops\\Amazon Linux.pem'
    }

    triggers {
        githubPush()
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
                    sh "docker build -t %IMAGE_NAME% ."
                    sh "docker run --rm %IMAGE_NAME% npm test"
                }
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                        docker push %IMAGE_NAME%
                    '''
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh """
                    plink -i \"%PEM_FILE%\" %EC2_HOST% ^
                    "docker pull %IMAGE_NAME% && docker stop nodeapp || true && docker rm nodeapp || true && docker run -d -p 80:3000 --name nodeapp %IMAGE_NAME%"
                """
            }
        }
    }

    post {
        failure {
            echo '❌ Pipeline failed.'
        }
        success {
            echo '✅ Build, test, push, and deploy completed!'
        }
    }
}

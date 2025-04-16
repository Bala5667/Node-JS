pipeline {
    agent any

    triggers {
        githubPush()
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
                    sh 'docker build -t balaji5667/node-js-app .'
                    sh 'docker run --rm balaji5667/node-js-app npm test'
                }
            }
        }

        stage('Docker Image') {
            steps {
                script{
                   sh 'docker images'
                }
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'balaji5667', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                        sh 'docker push balaji5667/node-js-app'
                    }
                }
            }
        }
    }
}

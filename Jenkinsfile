pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('balaji5667')
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
                    sh 'docker build -t balaji5667/node-js-app .'
                    sh 'docker run --rm balaji5667/node-js-app npm test'
                }
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'balaji5667', usernameVariable: '

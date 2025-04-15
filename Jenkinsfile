pipeline {
    agent any

    tools {
        git 'Default'  // This should match the name in Jenkins Git config
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
                bat 'npm install'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'npm test'
            }
        }
    }

    post {
        success {
            echo '✅ Build and test completed successfully!'
        }
        failure {
            echo '❌ Something went wrong during build or test.'
        }
    }
}

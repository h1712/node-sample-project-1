pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/h1712/node-sample-project-1.git'

            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test' // You can write some simple unit tests
            }
        }
        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        // Assuming AWS CLI is configured
                        sh 'aws s3 cp ./app.js s3://your-s3-bucket/ --region us-east-1'
                    }
                }
            }
        }
    }
}

#!/usr/bin/env groovy
pipeline {
    agent any

    // Configure NodeJS tool if available
    tools {
        nodejs "latest" // Update with your configured NodeJS tool name
    }

    environment {
        // Define any environment variables needed
        NODE_ENV = 'production'
    }

    stages {
        stage('Preflight') {
            steps {
                echo sh(returnStdout: true, script: 'env')
                sh 'node -v'
                sh 'npm --version'
            }
        }

        stage('Checkout') {
            steps {
                // Check out code from GitHub
                git branch: 'main', url: 'https://github.com/h1712/node-sample-project-1.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                // Show last git commit
                sh 'git log --reverse -1'
                // Run any build script (if present)
                sh 'npm run build' // Requires a "build" script in package.json
            }
        }

        stage('Test') {
            steps {
                // Run tests defined in package.json
                sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy only if the branch is 'main'
                    if (env.BRANCH_NAME == 'main') {
                        echo 'Deploying to S3 bucket...'
                        sh 'aws s3 cp ./app.js s3://your-s3-bucket/ --region us-east-1'
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean workspace after build
            cleanWs()
        }
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed. Check logs for details.'
        }
    }
}

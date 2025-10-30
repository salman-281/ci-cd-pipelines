pipeline {
    agent any

    environment {
        VERCEL_TOKEN = credentials('VERCEL_TOKEN')
        PATH = "C:\\Program Files\\nodejs;${env.PATH}"  // Add Node.js to PATH
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Verify Node.js') {
            steps {
                echo 'Verifying Node.js installation...'
                bat 'node -v'
                bat 'npm -v'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing Next.js dependencies...'
                bat 'npm ci'
            }
        }

        stage('Lint') {
            steps {
                echo 'Running Next.js lint...'
                bat 'npm run lint'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                bat 'npm run test'
            }
        }

        stage('Build Next.js') {
            steps {
                echo 'Building Next.js project...'
                bat 'npm run build'
            }
        }

        stage('Deploy to Vercel') {
            steps {
                echo 'Deploying Next.js app to Vercel...'
                bat 'npx vercel --token %VERCEL_TOKEN% --prod --confirm'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully and deployed to Vercel!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
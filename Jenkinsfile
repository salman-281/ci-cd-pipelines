pipeline {
    agent any

    environment {
        NODE_VERSION = '20' // Your Node.js version
        VERCEL_TOKEN = credentials('VERCEL_TOKEN') // Reference your Jenkins credential
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Setup Node.js') {
            steps {
                echo "Using Node.js ${NODE_VERSION}"
                nodejs(nodeJSInstallationName: 'NodeJS') {
                    sh 'node -v'
                    sh 'npm -v'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing Next.js dependencies...'
                sh 'npm ci'
            }
        }

        stage('Lint') {
            steps {
                echo 'Running Next.js lint...'
                sh 'npm run lint'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                sh 'npm run test'
            }
        }

        stage('Build Next.js') {
            steps {
                echo 'Building Next.js project...'
                sh 'npm run build'
            }
        }

        stage('Deploy to Vercel') {
            steps {
                echo 'Deploying Next.js app to Vercel...'
                sh '''
                npx vercel --token $VERCEL_TOKEN --prod --confirm
                '''
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

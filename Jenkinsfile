pipeline {
    agent any

    environment {
        VERCEL_TOKEN = credentials('VERCEL_TOKEN')
    }

    stages {
        stage('install') {
            steps {
                echo 'Checking out source code...'
                bat 'npm install'
            }
        }

        stage('test') {
            steps {
                echo 'Verifying Node.js installation...'
            }
        }

        stage('build') {
            steps {
                echo 'Building Next.js project...'
                bat 'npm run build'
            }
        }

        stage('deploy') {
            steps {
                echo 'Deploying Next.js app to Vercel...'
                bat 'npx vercel --token %VERCEL_TOKEN% --prod --confirm'
            }
        }

    }

}
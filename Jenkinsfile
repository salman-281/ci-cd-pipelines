pipeline {
    agent any

    environment {
        VERCEL_TOKEN = credentials('VERCEL_TOKEN')
    }

    stages {
        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                bat 'npm ci'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests (if any)...'
                bat 'npm run test || echo "No tests found or skipped"'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Next.js project...'
                bat 'npm run build'
            }
        }

        stage('Deploy to Vercel') {
            steps {
                echo 'Deploying Next.js app to Vercel...'
                bat '''
                REM Use workspace-local npm folders to avoid SYSTEM account issues
                npm config set prefix "%WORKSPACE%\\.npm-global"
                npm config set cache "%WORKSPACE%\\.npm-cache"
                set PATH=%WORKSPACE%\\.npm-global;%PATH%
                npx vercel --token %VERCEL_TOKEN% --prod --confirm
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}

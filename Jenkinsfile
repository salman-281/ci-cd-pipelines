pipeline {
    agent any

    environment {
        NODE_VERSION = '20' // Your Node.js version
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
                // Use NodeJS plugin or nvm on agent
                nodejs(nodeJSInstallationName: 'NodeJS') {
                    sh 'node -v'
                    sh 'npm -v'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing Next.js dependencies...'
                sh 'npm ci'  // clean install for reproducible builds
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

        stage('Optional: Export Static') {
            when {
                expression { return env.BUILD_TYPE == 'static' }
            }
            steps {
                echo 'Exporting static HTML for Next.js...'
                sh 'npm run export'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Next.js app...'
                // Example deployment commands:
                // sh 'scp -r .next/* user@server:/var/www/myapp'
                // Or trigger a Docker build/push
            }
        }
    }

    post {
        success {
            echo 'Next.js pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

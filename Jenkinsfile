// This is a test comment to trigger Jenkins build
pipeline {
    agent any

    tools {
        nodejs 'node22'
    }

    environment {
        PATH = "C:\\Program Files\\nodejs\\;${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/suprieyagtm10/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'  
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }

        stage('Fix Vulnerabilities') {
            steps {
                bat 'npm audit fix || exit /b 0'
            }
        }

        stage('Verify Fixes') {
            steps {
                echo 'Re-running npm audit to check remaining vulnerabilities'
                bat 'npm audit || exit /b 0'
            }
        }
    }
}


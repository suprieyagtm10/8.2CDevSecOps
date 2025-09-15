// This is the script for Jenkin
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
            post {
                always {
                    echo "Sending email after Run Tests stage..."
                    emailext(
                        subject: "Pipeline Stage: Run Tests - ${currentBuild.currentResult}",
                        body: """The Run Tests stage finished with status: ${currentBuild.currentResult}
Check console output at ${env.BUILD_URL}""",
                        to: 'suprieyagtm10@gmail.com',
                        from: 'suprieyagtm10@gmail.com',
                        attachLog: true
                    )
                }
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
            post {
                always {
                    echo "Sending email after Security Scan stage..."
                    emailext(
                        subject: "Pipeline Stage: Security Scan - ${currentBuild.currentResult}",
                        body: """The Security Scan stage finished with status: ${currentBuild.currentResult}
Check console output at ${env.BUILD_URL}""",
                        to: 'suprieyagtm10@gmail.com',
                        from: 'suprieyagtm10@gmail.com',
                        attachLog: true
                    )
                }
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

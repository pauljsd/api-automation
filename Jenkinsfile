pipeline {
    agent any

    triggers {
        cron('H 7,13 * * *')   // Runs twice daily: 7AM & 1PM
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/pauljsd/api-automation.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
                sh 'npm install -g newman-reporter-htmlextra'
            }
        }
         stage('Prepare Reports Directory') {
            steps {
                // Make sure reports folder exists
                sh 'mkdir -p reports'
            }
        }
        stage('Run API Tests') {
            steps {
                sh 'npm run test-api'
            }
        }
        stage('Publish Report') {
            steps {
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'reports',
                    reportFiles: 'report.html',
                    reportName: 'API Test Report'
                ])
            }
        }
    }
}

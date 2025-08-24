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
                bat 'npm install'
                bat 'npm install -g newman newman-reporter-htmlextra'
                bat 'where newman' // Confirm path to newman in Windows
            }
        }

        stage('Prepare Reports Directory') {
            steps {
                bat '''
                if not exist reports (
                    mkdir reports
                )
                '''
            }
        }

        stage('Run API Tests') {
            steps {
                // Run tests without failing pipeline if they fail
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    // Use full path to newman if needed (replace with your output from `where newman`)
                    bat 'newman run collections/API_AUTOMATION.postman_collection.json -e environments/goRestStaging.postman_environment.json -r htmlextra --reporter-htmlextra-export reports/report.html'
                }
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

    post {
        always {
            echo "Pipeline finished. Check HTML report above."
        }
    }
}

pipeline {
    agent any
    stages {
        stage('Install') {
            steps {
                
                bat 'pip install robotframework'
            }
        }
        stage('Build') {
            steps {
                
                bat 'robot --name Robot --loglevel DEBUG --outputdir results keyword_driven.robot data_driven.robot gherkin.robot'
            }
        }
        stage('Publish Reports') {
            steps {
                junit 'results/*.xml'
            
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'results',
                    reportFiles: 'report.html',
                    reportName: 'Robot Framework Results'
                ])
            }
        }
    }
}

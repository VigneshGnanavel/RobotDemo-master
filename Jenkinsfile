pipeline {
    agent any

    environment {
        JIRA_AUTH_TOKEN = credentials('jenkins')
    }

    stages {
        stage('Install') {
            steps {
                bat 'pip install robotframework'
            }
        }
        stage('Run Tests') {
            steps {
                bat 'robot --name Robot --loglevel DEBUG --outputdir results keyword_driven.robot data_driven.robot gherkin.robot'
            }
        }
        stage('Publish Reports') {
            steps {
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'results',
                    reportFiles: 'report.html',
                    reportName: 'Robot Framework Report'
                ])
            }
        }
        stage('Xray Import') {
            steps {
                script {
                    def filePath = 'C:/ProgramData/Jenkins/.jenkins/workspace/robot_pipeline/results/output.xml'
                    def issueKey = "TA-3"
                    def jiraUrl = "https://gnanavelvignesh183-1718958763592.atlassian.net/rest/api/2/import/execution/${issueKey}"
                    bat "curl -D- -u $JIRA_AUTH_TOKEN -X POST --data-binary @$filePath -H \"Content-Type: application/xml\" $jiraUrl"
                }
            }
        }
    }
}

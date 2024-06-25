pipeline {
    agent any

    environment {
        JIRA_BASE_URL = 'https://gnanavelvignesh183-1718958763592.atlassian.net'
        JIRA_PROJECT_KEY = 'TA' 
        JIRA_ISSUE_KEY = 'TA-3'
        JIRA_API_TOKEN = credentials('jenkins')
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
                    def filePath = 'results/output.xml'
                    def jiraUrl = "${JIRA_BASE_URL}/rest/api/2/import/execution/${issueKey}"

                    bat "curl -D- -u $USERNAME:$PASSWORD -X POST --data-binary @$filePath -H 'Content-Type: application/xml' $jiraUrl"
                }
            }
        }
    }
}

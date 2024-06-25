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
                    reportDir: 'C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results',
                    reportFiles: 'report.html',
                    reportName: 'Robot Framework Report'
                ])
            }
        }

        stage('Xray Import') {
            steps {
                script {
                    def filePath = 'C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\output.xml'
                    if (fileExists(filePath)) {
                        def fileContent = readFile(filePath)
                        withCredentials([string(credentialsId: 'jenkins')]) {
                            def response = httpRequest(
                                acceptType: 'APPLICATION_JSON',
                                contentType: 'APPLICATION_XML',
                                httpMode: 'POST',
                                requestBody: fileContent,
                                url: 'https://gnanavelvignesh183-1718958763592.atlassian.net/rest/api/2/import/execution/junit?testExecKey=TA-3',
                                customHeaders: [
                                    [name: 'Authorization', value: "Basic ${JIRA_AUTH_TOKEN}"],
                                    [name: 'Content-Type', value: 'application/xml']
                                ]
                            )
                            echo "Response: ${response.status} - ${response.content}"
                        }
                    } else {
                        error "File not found: ${filePath}"
                    }
                }
            }
        }
    }
}

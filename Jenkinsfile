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
        
        stage('Git Commit and Push') {
            steps {
                script {
                    bat 'git config --global user.name "VigneshGnanavel"'
                    bat 'git config --global user.email "prathvikvignesh@gmail.com"'
                    
                    bat 'git checkout results'
                    
                    bat 'dir "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results"'

                    bat 'git add -f "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\output.xml"'
                    bat 'git add -f "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\log.html"'
                    bat 'git add -f "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\report.html"'
                    
                    bat 'git commit -m "Add test results"'
                    bat 'git push origin results'
                }
            }
        }
        
        stage('Xray Import') {
            steps {
                script {
                    def response = httpRequest(
                        acceptType: 'APPLICATION_JSON',
                        contentType: 'APPLICATION_XML',
                        httpMode: 'POST',
                        requestBody: readFile('C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\.xml'),
                        url: 'https://gnanavelvignesh183-1718958763592.atlassian.net/rest/api/2/import/execution/junit?testExecKey=TA-3',
                        customHeaders: [
                            [name: 'Authorization', value: "Basic ${env.JIRA_AUTH_TOKEN}"],
                            [name: 'Content-Type', value: 'application/xml']
                        ]
                    )
                    echo "Response: ${response.status} - ${response.content}"
                }
            }
        }
    }
}

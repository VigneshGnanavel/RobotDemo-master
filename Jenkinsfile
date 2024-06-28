pipeline {
    agent any
    environment {
        JIRA_AUTH_TOKEN = credentials('jira_new')
    }
    stages {
        stage('Install Dependencies') {
            steps {
                // Install Robot Framework
                bat 'pip install robotframework'
            }
        }
        
        stage('Run Tests') {
            steps {
                // Run Robot Framework tests and generate outputs
                bat 'robot --name Robot --loglevel DEBUG --outputdir results keyword_driven.robot data_driven.robot gherkin.robot'
            }
        }
        
        stage('Publish Reports') {
            steps {
                // Publish HTML reports generated by Robot Framework
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
                    
                    bat 'git checkout -B results'
                    
                    bat 'dir "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results"'

                    bat 'git add -f "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\output.xml"'
                    bat 'git add -f "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\log.html"'

                    bat 'if exist "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\report.html" git add -f "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\report.html"'

                    bat 'git add .'

                    bat 'git commit -m "inital commit"'

                    bat 'git push origin results'
                }
            }
        }
        stage('Xray Import') {
            steps {
                script {
                    def filePath = 'C:/ProgramData/Jenkins/.jenkins/workspace/robot_pipeline/results/output.xml'
                    def issueKey = "TA-8"
                    def jiraUrl = "https://gnanavelvignesh124.atlassian.net/rest/api/2/import/execution/$issueKey"
                    bat "curl -D- -u $JIRA_AUTH_TOKEN -X POST --data-binary @$filePath -H \"Content-Type: application/xml\" $jiraUrl"
                }
            }
        }

    }
}

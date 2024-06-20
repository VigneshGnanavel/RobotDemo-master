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
        
        stage('Git Reports') {
            steps {
                
                script {
                    bat 'git checkout -B results'
                }
                
                
                bat 'git config --global user.name "VigneshGnanavel"'
                bat 'git config --global user.email "prathvikvignesh@gmail.com"'
                
                
                bat 'git add .'
                
                bat 'git push origin results'
            }
        }
    }
}

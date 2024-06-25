pipeline {
    agent any
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
        stage('Git Commit and Push') {
            steps {
                script {
                    bat 'git config --global user.name "VigneshGnanavel"'
                    bat 'git config --global user.email "prathvikvignesh@gmail.com"'
                    
                    bat 'git checkout TA-3-junitresults'
                    
                    bat 'dir "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results"'
                    
                    bat 'git add -f "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\log.html"'
                    bat 'git add -f "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\output.xml"'
                    bat 'git add -f "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\report.html"'
                    
       
                    bat 'git add .'

                    bat 'git commit -m "newbranchadded"'
                    
                    bat 'git push origin TA-3-junitresults'
                }
            }
        } 
    } 
} 

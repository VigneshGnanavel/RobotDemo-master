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
        stage('Deploy to GitHub Pages') {
            steps {
                // Checkout the repository using the gitdemo_pat credentials
                git credentialsId: 'gitdemo_pat', url: 'https://github.com/VigneshGnanavel/RobotDemo-master.git'
                
                // Copy the HTML report to the repository
                bat 'xcopy /s "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\report.html" .\\docs\\'
                
                // Push changes to GitHub
                bat 'git add .'
                bat 'git commit -m "Add Robot Framework report"'
                bat 'git push origin main'
            }
        }
    }
}

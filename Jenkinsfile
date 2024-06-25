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
                    
                    // Add and commit changes
                    bat 'git add results/log.html results/output.xml results/report.html'
                    bat 'git commit -m "Committing changes"'
                    
                    // Checkout the branch
                    bat 'git checkout TA-3-junitresults'
                    
                    // Push changes to TA-3-junitresults branch
                    bat 'git push origin TA-3-junitresults'
                }
            }
        }
    }
}

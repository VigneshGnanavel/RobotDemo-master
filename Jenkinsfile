pipeline {
    agent any
    
    environment {
        ARTIFACTORY_ACCESS_TOKEN = credentials('artifactory-access-token')
    }
    
    stages {
        stage('Install Dependencies') {
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
                    
                    bat 'git checkout -B results'
                    
                    bat 'dir "results"'
                    
                    bat 'git add -f "results/output.xml"'
                    bat 'git add -f "results/log.html"'
                    
                    bat 'if exist "results/report.html" git add -f "results/report.html"'
                    
                    bat 'git commit -m "Initial commit"'
                    
                    bat 'git push origin results'
                }
            }
        }
        
        stage('Upload Artifact to Artifactory') {
            steps {
                script {
                    bat "jfrog rt upload --url http://172.17.208.1:8082/artifactory/ --access-token ${ARTIFACTORY_ACCESS_TOKEN} results/output.xml results/"
                }
            }
        }
    }
}


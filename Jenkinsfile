pipeline {
    agent any
    
    environment {
        CI = true
        ARTIFACTORY_ACCESS_TOKEN = credentials('artifactory-access-token')
        JAVA_HOME = 'C:\\Program Files\\Eclipse Adoptium\\jdk-11.0.23.9-hotspot'
        PATH = "${env.JAVA_HOME}\\bin;${env.PATH}"
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
        
        stage('Upload Artifact to Artifactory') {
            steps {
                script {
                    bat "jf rt upload --url http://172.17.208.1:8082/artifactory/ --access-token ${ARTIFACTORY_ACCESS_TOKEN} results/output.xml results/"
                }
            }
        }
    }
}

pipeline {
    agent any
    stages {
        stage('Install') {
            steps {
                script {
                    sh '''
                    pip install robotframework
                    '''
                }
            }
        }
        stage('Build'){
            steps {
                sh '''
                robot --name Robot --loglevel DEBUG keyword_driven.robot data_driven.robot gherkin.robot
                '''
            }
        }
    }
}
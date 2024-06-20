pipeline {
    agent any
    stages {
        stage('Install') {
            steps {
                script {
                    bat '''
                    python -m venv venv
                    venv\\Scripts\\activate
                    pip install --upgrade pip
                    pip install robotframework
                    '''
                }
            }
        }
        stage('Build'){
            steps {
                script {
                    bat '''
                    venv\\Scripts\\activate
                    robot --name Robot --loglevel DEBUG keyword_driven.robot data_driven.robot gherkin.robot
                    '''
                }
            }
        }
    }
}

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
    }
}

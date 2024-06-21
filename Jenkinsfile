pipeline {
    agent any
    
    stages {
        stage('Install Dependencies') {
            steps {
                bat 'pip install robotframework'
                bat 'pip install robotframework-junitxml'
            }
        }
        
        stage('Run Tests') {
            steps {
                bat 'robot --name Robot --loglevel DEBUG --outputdir results keyword_driven.robot data_driven.robot gherkin.robot'
                // Convert output to JUnit XML
                bat 'rebot --outputdir results --output results/output.xml --report results/report.html --log results/log.html'
                bat 'python -m robot.jupyter junitxml --output results/output.xml --resultfile results/junit_output.xml'
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
        
        stage('Git Commit and Push') {
            steps {
                script {
                    bat 'git config --global user.name "VigneshGnanavel"'
                    bat 'git config --global user.email "prathvikvignesh@gmail.com"'
                    
                    bat 'git checkout -B results'
                    
                    bat 'dir "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results"'
                    
                    bat 'git add -f "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\output.xml"'
                    bat 'git add -f "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\log.html"'
                    bat 'git add -f "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\report.html"'
                    bat 'git add -f "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\junit_output.xml"'
                    
                    bat 'git commit -m "Automated commit of test results"'
                    bat 'git push origin results'
                }
            }
        }

        stage('Upload Results to Xray') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'jira_id', passwordVariable: 'XRAY_CLIENT_SECRET', usernameVariable: 'XRAY_CLIENT_ID')]) {
                    script {
                        def xrayApiBaseUrl = 'https://xray.cloud.xpand-it.com'
                        def junitFile = "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\robot_pipeline\\results\\junit_output.xml"

                        def response = httpRequest(
                            httpMode: 'POST',
                            url: "${xrayApiBaseUrl}/api/v1/import/execution/junit",
                            customHeaders: [
                                [name: 'Authorization', value: "Bearer ${XRAY_CLIENT_ID}:${XRAY_CLIENT_SECRET}"],
                                [name: 'Content-Type', value: 'application/xml']
                            ],
                            requestBody: readFile(junitFile)
                        )

                        echo "Xray response: ${response}"
                    }
                }
            }
        }
    }
}

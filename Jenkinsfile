pipeline {
    agent { label 'nodejs-agent1'}  // Define the agent (can be any available node)
    environment{
        PROJECT = 'Expense'
        COMPONENT = 'Backend'
        appVersion = ''
    }
    options{
        disableConcurrentBuilds()
        timeout(time: 30, unit: 'MINUTES')
    }
    stages {
        stage('Read version') {
            steps {
                script {
                    def packageJSON = readJSON file: 'package.json'
                    appVersion = packageJSON.version
                    echo "Appversion is: $appVersion"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh """
                        npm install
                    """
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    sh """
                        docker build -t backend:v1.0.0 .
                    """ 
                }
            }
        }
    }

    post {
        always {
            // Clean up or notify team
            echo 'Pipeline complete' 
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

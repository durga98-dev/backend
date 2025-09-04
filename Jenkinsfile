pipeline {
    agent { label 'nodejs-agent1'}  // Define the agent (can be any available node)
    environment{
        PROJECT = 'Expense'
        COMPONENT = 'Backend'
        appVersion = ''
    }
    options{
        disableConcurrentBuilds()
        timeout(time: 5, unit: 'SECONDS')
    }
    stages {
        stage('Read version') {
            steps {
                script {
                    def packageJSON = readJSON file: 'package.json'
                    appVersion = packageJSON.version
                }
            }
        }

        stage('Test') {
            steps {
                // Run unit tests
                script {
                    sh """
                        echo "Hello, this is Test"
                    """ 
                }
            }
        }

        stage('Deploy') {
            steps {
                // Deploy the application (for example, to a staging environment)
                script {
                    sh """
                        echo "Hello, this is Deploy"
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

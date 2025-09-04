pipeline {
    agent { label 'nodejs-agent1'}  // Define the agent (can be any available node)
    environment{
        PROJECT = 'expense'
        COMPONENT = 'backend'
        appVersion = ''
        ACC_ID = '975050030597'
    }
    options{
        disableConcurrentBuilds()
        timeout(time: 30, unit: 'MINUTES')
        ansiColor('xterm')
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

        stage('Docker Build Image') {
            steps {
                script {
                    withAWS(region: 'us-east-1', credentials: 'aws-creds'){
                    sh """
                        aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com

                        docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .

                        docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                    """ 
                    }
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

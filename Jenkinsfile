pipeline {

    agent {
        label 'AGENT-1'
    }
    
    environment {
        appVersion = ''

    }

    stages {
        stage('read packagejson') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "application version: ${appVersion}"

                }
                
            }
        }

        stage('building the image') {
            steps {
                script {
                    withAWS(credentials: 'aws-auth', region: 'us-east-1')
                }
            }
        }
    }



}
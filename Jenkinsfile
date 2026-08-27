pipeline {

    agent {
        label 'AGENT-1'
    }
    
    environment {
        appVersion = ''
        REGION = "us-east-1"
        COMPONENT = "catalogue"
        ACC_ID = "657082817363"
        PROJECT = "roboshop"

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
                    withAWS(credentials: 'aws-auth', region: 'us-east-1') {
                        sh """
                            aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                            docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
                            docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                        """

                    }
                  
                }
            }
        }
    }



}
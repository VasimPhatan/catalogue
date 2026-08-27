pipeline {

    agent {
        label 'AGENT-1'
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
    }



}
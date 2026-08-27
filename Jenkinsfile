pipeline {

    agent {
        label 'AGENT-1'
    }


    stages {
        stage('read packagejson') {
            steps {
                def packageJson = readJSON file: 'package.json'
                appVersion = packageJson.appVersion
                echo "application version: ${appVersion}"

            }
        }
    }



}
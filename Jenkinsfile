pipeline {

    agent { node { label 'agent1'} }

    stages {
        stage('install dependencies') {

            steps {
                sh '''
                    pwd 
                    ls -ltrh
                    npm install
                '''
            }

        }

        stage('unit testing') {
            steps {
                echo " this stage is used  for unit testing"
            }
        }

        stage('sonar scanning') {
            steps {
                sh '''
                sonar-scanner
                '''
            }
        }
    }
}
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

        // stage('sonar scanning') {
        //     steps {
        //         sh '''
        //         sonar-scanner
        //         '''
        //     }
        // }

        stage('building the code') {
            steps {
                sh '''
                zip -r catalogue.zip ./* --exclude=.zip --exclude=.git
                pwd 
                ls -ltrh 
                '''
            }
        }

    }

    post {
        always {
            echo "cleaning the workspace"
            deleteDir()
        }
    }
}
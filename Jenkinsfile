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

          stage('pushing the artifacts to nexus') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '172.31.92.44:8081/',  //give nexus url and port 
                    groupId: 'com.roboshop',    //give group id 
                    version: "1.0.1",         // give version number 
                    repository: 'catalogue',   //repository name 
                    credentialsId: 'nexus-auth', //for credentails create credentials in jenkins for nexus and give name in here.
                    artifacts: [
                        [artifactId: 'catalogue', 
                        classifier: '',
                        file: 'catalogue.zip',  // artifact that needs to pushed to nexus 
                        type: 'zip'] //type of artifact
                    ]
                )
            }
        }

        

    }

    post {
        always {
            echo "cleaning the workspace"
            //deleteDir()
        }
    }
}
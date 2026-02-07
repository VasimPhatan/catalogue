pipeline {

    agent { node { label 'agent1'} }

    stages {
        stage('install dependencies') {

            steps {
                sh '''
                    pwd 
                    ls -ltrh
                '''
            }

        }
    }
}
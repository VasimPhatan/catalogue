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

        stage('Install dependencies') {
            steps {
                script {
                    sh """
                     npm install
                    """
                }
            }
        }

        stage('unit testing') {
            steps {
                script {
                    sh """
                      echo "unit testing"
                    """
                }
            }
        }


        stage('sonar scan') {
            environment {  //here we passing sonar tool 
                scannerHome = tool 'sonar-8.1'
            }

            steps {
                script {
                    //sonar server environment.
                    withSonarQubeEnv( InstallationName: 'sonar-8.1') { // this will inject the sonar server and authentication all these will inject 
                    sh "${scannerHome}/bin/sonar-scanner"  //in order to run this we need sonar-rproject.properties this will give by developers
                    } 

                }
            }
        }


        stage("Quality Gate Check") {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
            // Automatically fails and halts the build if status is not 'OK'
                waitForQualityGate abortPipeline: false // for first 2 week i will make false 
                }
            }
        }


        stage('Check Dependabot Alerts') {
            environment { 
                GITHUB_TOKEN = credentials('git-token')
            }
            steps {
                script {
                    // Fetch alerts from GitHub
                    def response = sh(
                        script: """
                            curl -s -H "Accept: application/vnd.github+json" \
                                 -H "Authorization: token ${GITHUB_TOKEN}" \
                                 https://api.github.com/repos/VasimPhatan/catalogue/dependabot/alerts
                        """,
                        returnStdout: true
                    ).trim()

                    // Parse JSON
                    def json = readJSON text: response

                    // Filter alerts by severity
                    def criticalOrHigh = json.findAll { alert ->
                        def severity = alert?.security_advisory?.severity?.toLowerCase()
                        def state = alert?.state?.toLowerCase()
                        return (state == "open" && (severity == "critical" || severity == "high"))
                    }

                    if (criticalOrHigh.size() > 0) {
                        error "❌ Found ${criticalOrHigh.size()} HIGH/CRITICAL Dependabot alerts. Failing pipeline!"
                    } else {
                        echo "✅ No HIGH/CRITICAL Dependabot alerts found."
                    }
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
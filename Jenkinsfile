pipeline {
    agent {
        node {
            label 'agent-1'
        }
    }
    environment {
        AWS_REGION = 'us-east-1'
        PROJECT = 'roboshop'
        COMPONENT = 'catalogue'
        AWS_ACCOUNT_ID = '368903465982'

    }
    stages {
        stage ('read JSON') {
            steps {
                script {
                    sh 'ls -l'
                    def jsonData = readJSON file: 'package.json'
                    env.appVersion = jsonData.version
                    echo "Application Version: ${env.appVersion}"
                }                
            }
        }
        stage ('Install Dependencies') {
            steps {
                sh 'npm install'    
            }
        }
        stage ('Build Docker image') {
            steps {
                script {
                    withAWS(region:'us-east-1',credentials:'aws-auth') {
                        sh """
                            aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                            docker build -t ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${PROJECT}/${COMPONENT}:${env.appVersion} .
                            docker images
                            docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${PROJECT}/${COMPONENT}:${env.appVersion}

                        """
                    }
                }
            }
        }
    }
    // Post build section
    post {
        always {
            echo "Cleaning up workspace..."
            cleanWs()
        }
        success {
            echo "This build is successful. Version: ${env.appVersion}"
        }
    }
}
pipeline {
    agent {
        node{
            label 'agent-1'
        }
    }
    environment {
        appVersion = ''
    }
    stages {
        stage ('read JSON') {
            steps {
                script {
                    def jsonData = readJson file: 'package.json'
                    appVersion = jsonData.version
                }                
            }
        }
        stage ('Build') {
            steps {
                sh 'npm install'
            }
        }
    }
    post {
        always {
            echo "Cleaning up workspace..."
            cleanWs()
        }
        success {
            echo "This build is successful. Version: ${appVersion}"
        }
    }
}
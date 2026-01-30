pipeline {
    agent {
        node{
            label 'agent-1'
        }
    }
    environment {
        appVersion = ""
    }
    stages {
        stage ('read JSON') {
            steps {
                script {
                    def jsonData = readJson file: 'package.json'
                    appVersion = jsonData.version
                    echo "Application Version: ${appVersion}"
                }                
            }
        }
        stage ('Install Dependencies') {
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
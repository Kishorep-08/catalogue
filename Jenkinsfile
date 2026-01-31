pipeline {
    agent {
        node {
            label 'agent-1'
        }
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
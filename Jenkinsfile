pipeline {
    agent {
        node{
            lable 'agent-1'
        }
    }
    environment {
        appVersion = ''
    }
    stages {
        stage ('read JSON') {
            steps {
                def jsonData = readJson file: 'package.json'
                appVersion = jsonData.version
            }
        }
        stage ('Build') {
            steps {
                sh 'npm install'
            }
        }
    }
}
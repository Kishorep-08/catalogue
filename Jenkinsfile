pipeline {
    agent {
        node {
            label 'agent-1'
        }
    }
    environment {
        appVersion = ''
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
        stage ('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage ('Depandabot scan'){
            environment {
                GITHUB_API = 'https://api.github.com'
                GITHUB_OWNER = 'kishorep-08'
                GITHUB_REPO = 'catalogue'
                GITHUB_TOKEN = credentials('github-token')
            }
            steps {
                script {
                    sh '''
                    echo "Fetching Dependabot alerts..."

                    response=$(curl -s \
                        -H "Authorization: token ${GITHUB_TOKEN}" \
                        -H "Accept: application/vnd.github+json" \
                        "${GITHUB_API}/repos/${GITHUB_OWNER}/${GITHUB_REPO}/dependabot/alerts")

                    echo "${response}" > dependabot_alerts.json

                    high_critical_open_count=$(echo "${response}" | jq '[.[] 
                        | select(
                            .state == "open"
                            and (.security_advisory.severity == "high"
                                or .security_advisory.severity == "critical")
                        )
                    ] | length')

                    echo "Open HIGH/CRITICAL Dependabot alerts: ${high_critical_open_count}"

                    if [ "${high_critical_open_count}" -gt 0 ]; then
                        echo "❌ Blocking pipeline due to OPEN HIGH/CRITICAL Dependabot alerts"
                        echo "Affected dependencies:"
                        echo "$response" | jq '.[] 
                        | select(.state=="open" 
                        and (.security_advisory.severity=="high" 
                        or .security_advisory.severity=="critical"))
                        | {dependency: .dependency.package.name, severity: .security_advisory.severity, advisory: .security_advisory.summary}'
                        exit 1
                    else
                        echo "✅ No OPEN HIGH/CRITICAL Dependabot alerts found"
                    fi
                    '''
                }
            }

        }


        /* stage ('Build Docker image') {
            steps {
                script {
                    withAWS(region:'us-east-1',credentials:'aws-auth') {
                        sh """
                            aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                            docker build -t ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
                            docker images
                            docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}

                        """
                    }
                }
            }
        }
     */
    }
    // Post build section
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
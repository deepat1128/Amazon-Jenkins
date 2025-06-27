  pipeline {
    agent any
        stages {
        stage('Checkout Code') {
            steps {
                // Checkout your actual GitHub repo and correct branch with credentials
                git url: 'https://github.com/deepat1128/Amazon-Jenkins', branch: 'feature', credentialsId: 'deepaoum1128@gmail.com'
            }
        }
        stage('compile') {
            steps {
                script {
                    echo "Starting Maven Build..."
                    sh 'mvn compile'
                }
            }
        }

        stage('Build Project') {
            steps {
                script {
                    echo "Starting Maven Build..."
                    sh 'mvn clean install'
                }
            }
        }
  stage('Send Build Started Notification') {
            steps {
                script {
                    def slack_Webhook_Url = 
'https://hooks.slack.com/services/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' 
                    def message = '{"text": "Jenkins Build Started for Amazon Project."}'

                    sh '''
  curl -X POST -H 'Content-type: application/json'  
       --data "${message}" 
       ${slack_Webhook_Url}
'''

                }
            }
        }
    }
post {
  
        success {
          environment {
  message = '{"text": "Jenkins Build SUCCESS for Amazon Project."}'
  slack_Webhook_Url = 'https://hooks.slack.com/services/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'
}

            script {
                def slack_Webhook_Url = 'https://hooks.slack.com/services/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'
                def message = '{"text": "Jenkins Build SUCCESS for Amazon Project."}'

                sh '''
  curl -X POST -H 'Content-type: application/json'  
       --data "${message}" 
       ${slack_Webhook_Url}
'''

            }
        }

        failure {
            script {
                def slack_Webhook_Url = 'https://hooks.slack.com/services/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'
                def message = '{"text": "Jenkins Build FAILED for Amazon Project."}'

                sh '''
  curl -X POST -H 'Content-type: application/json'  
       --data "${message}" 
       ${slack_Webhook_Url}
'''

            }
        }
  always {
            echo "Build completed — Slack notified."
        }
    }
}

        

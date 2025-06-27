  pipeline {
    agent any
    environment {
    SLACK_WEBHOOK_URL = 'https://hooks.slack.com/services/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'
}
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
                    
                    sh """
  curl -X POST -H 'Content-type: application/json'\\  
       --data '{"text": "Build notification from Jenkins"}'\\ 
       ${SLACK_WEBHOOK_URL}
"""

                }
            }
        }
    }
post {
  
        success {
          

            script {
                
                sh """
  curl -X POST -H 'Content-type: application/json' \\
       --data '{"text": "Build successful"}' \\
       ${SLACK_WEBHOOK_URL}
"""

            }
        }

        failure {
            script {
                
                sh """
  curl -X POST -H 'Content-type: application/json' \\ 
       --data '{"text": "Build failure"}' \\
       ${SLACK_WEBHOOK_URL}
"""

            }
        }
  always {
            echo "Build completed — Slack notified."
        }
    }
}

        

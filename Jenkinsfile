pipeline {
    agent any
    environment {
        PATH = "/usr/bin/var/libs/jenkins/:$PATH"  // Explicitly add /usr/bin and other necessary paths
    }
    stages {
        stage('Checkout Code') {
            steps {
                // Pull the code from GitHub repository
                git credentialsId: '19f61618-c255-4ff6-bdb0-29e90103adaa', url: 'https://github.com/deepat1128/Amazon-Jenkins', branch:'feature'  // Use the desired branch
            }
        }

        stage('Build') {
            steps {
                script {
                    // Running the Maven build command
                    echo "Building the project..."
                    sh 'mvn clean install'
                }
            }
        }

        stage('slackSend(channel: '#jenkins-integration', message: 'Build SUCCESS', color: 'good', tokenCredentialId: 'A0932P68MFY'') {
            steps {
                script {
                    def slackWebhookUrl = 'https://hooks.slack.com/services/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'
                    
                    // Determine the build status
                    def buildStatus = currentBuild.result ?: 'SUCCESS'  // Default to SUCCESS if not explicitly set

                    // Create message based on build result
                    def message = """
                    {
                        "text": "Jenkins Build Status: ${buildStatus}",
                        "attachments": [
                            {
                                "text": "Build ${buildStatus}",
                                "color": "${buildStatus == 'SUCCESS' ? 'good' : 'danger'}"
                            }
                        ]
                    }
                    """
                    
                    // Send Slack notification with build status
                    sh """
                    curl -X POST -H 'Content-type: application/json' --data '${message}' ${slackWebhookUrl}
                    """
                }
            }
        }
    }
    post {
        always {
            // This block ensures that a message is sent regardless of success or failure
            script {
                def buildStatus = currentBuild.result ?: 'SUCCESS'
                def slackWebhookUrl = 'https://hooks.slack.com/services/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'
                def message = """
                {
                    "text": "Jenkins Build Status: ${buildStatus}",
                    "attachments": [
                        {
                            "text": "Build ${buildStatus}",
                            "color": "${buildStatus == 'SUCCESS' ? 'good' : 'danger'}"
                        }
                    ]
                }
                """
                
                // Send the build status notification to Slack
                sh """
                curl -X POST -H 'Content-type: application/json' --data '${message}' ${slackWebhookUrl}
                """
            }
        }
    }
}


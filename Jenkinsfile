pipeline {
    agent any
    environment {
        PATH = "/usr/bin:/bin:/opt/homebrew/bin:$PATH"  // Explicitly add /usr/bin and other necessary paths
    }
    stages {
        stage('Checkout Code') {
            steps {
                // Pull the code from GitHub repository
                git url: 'https://github.com/PraveenKuber/Amazon', branch: 'main'  // Use the desired branch
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

        stage('Send Slack Notification via curl') {
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


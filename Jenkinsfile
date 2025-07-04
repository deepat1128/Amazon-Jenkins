 pipeline {
    agent any
    environment {
        PATH = "/usr/bin:$PATH"  
	// Explicitly add /usr/bin and other necessary paths
    }
    stages {
        stage('Checkout Code') {
            steps {
                // Pull the code  from the GitHub repository
                git url: 'https://github.com/deepat1128/Amazon-Jenkins', branch: 'feature'  
		// Use this desired branch
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
            // This block ensures that a message is been sent regardless of success or failure
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

  

pipeline {
    agent { 
        node { label 'windows' } 
    }

    stages {
        stage('Checkout') {
            steps {
                // FIXED: Correct syntax for checking out a specific branch and repository URL
                git branch: 'main', url: 'https://github.com/saaadhyaaa/jenkins7.git'
            }
        }

        stage('Build') {
            steps {
                // Compiles app.py to check for syntax errors
                bat 'python -m py_compile app.py'
                
                echo 'Waiting 15 seconds...'
                sleep time: 15, unit: 'SECONDS'
                
                // Newer builds reaching this point will cancel older builds
                milestone(1)
            }
        }

        stage('Send Notification') {
            steps {
                script {
                    def mailSubject = "Build Notification: ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}"
                    def mailBody = "The build completed. View details here: ${env.BUILD_URL}"
                    def recipient = "your-email@example.com" // Replace with your actual email

                    try {
                        mail to: recipient,
                             subject: mailSubject,
                             body: mailBody
                    } catch (Exception e) {
                        echo "=== SMTP NOT CONFIGURED (FALLBACK WORKAROUND) ==="
                        echo "To: ${recipient}"
                        echo "Subject: ${mailSubject}"
                        echo "Body: ${mailBody}"
                        echo "================================================="
                    }
                }
            }
        }
    }
}

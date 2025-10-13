pipeline {
    agent any

    stages {
        stage('Test Email') {
            steps {
                script {
                    echo "Testing email with system configuration only..."
                    
                    emailext (
                        to: 'amina1daghari@gmail.com',
                        subject: "TEST - ${env.BUILD_NUMBER}",
                        body: "This is a test email using Jenkins system configuration",
                        mimeType: 'text/html'
                    )
                    
                    echo "Email sent using system configuration"
                }
            }
        }
    }
}

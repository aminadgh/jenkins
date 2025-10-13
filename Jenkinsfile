pipeline {
    agent any

    stages {
        stage('Test Email with Debug') {
            steps {
                script {
                    echo "Current build environment:"
                    echo "BUILD_NUMBER: ${env.BUILD_NUMBER}"
                    echo "JOB_NAME: ${env.JOB_NAME}"
                    
                    // Test with minimal configuration first
                    emailext (
                        to: 'amina1daghari@gmail.com',
                        subject: "Minimal Test - ${env.BUILD_NUMBER}",
                        body: """
                            <h3>Test Email</h3>
                            <p>Build: ${env.BUILD_NUMBER}</p>
                            <p>Job: ${env.JOB_NAME}</p>
                        """,
                        mimeType: 'text/html'
                    )
                    
                    echo "Email function completed"
                }
            }
        }
    }
    
    post {
        always {
            echo "Build completed - check email status"
        }
    }
}

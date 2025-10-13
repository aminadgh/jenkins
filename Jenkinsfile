pipeline {
    agent any
    
    stages {
        stage('Test Email Unique') {
            steps {
                script {
                    echo "🚀 TEST EMAIL UNIQUE..."
                    
                    // Test le plus simple possible
                    emailext (
                        subject: "TEST ULTRA SIMPLE - ${new Date().format('HH:mm:ss')}",
                        body: "Ceci est un test ultra simple.",
                        to: "amina1daghari@gmail.com",
                        mimeType: "text/plain"
                    )
                    
                    echo "✅ Email envoyé (selon Jenkins)"
                }
            }
        }
    }
    
    post {
        always {
            echo "📋 Résultat: ${currentBuild.currentResult}"
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Test Configuration Complète') {
            steps {
                script {
                    echo "🔍 Test configuration SMTP complète..."
                    
                    emailext (
                        to: 'amina1daghari@gmail.com',
                        subject: "TEST COMPLET - ${env.BUILD_NUMBER}",
                        body: """
                        Test avec configuration SMTP complète
                        Build: ${env.BUILD_NUMBER}
                        Time: ${new Date()}
                        """,
                        mimeType: 'text/plain',
                        // Configuration SMTP explicite
                        smtpHost: 'smtp.gmail.com',
                        smtpPort: '587',
                        smtpAuth: 'true',
                        smtpStartTls: 'true',
                        // Authentification
                        authentication: 'amina1daghari@gmail.com',
                        password: 'drgb csjs hjbn dedj'  // Remplacez par votre vrai mot de passe d'application
                    )
                    
                    echo "✅ Email avec config complète envoyé"
                }
            }
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Test Email avec Auth Forcée') {
            steps {
                script {
                    echo "🔍 Test avec authentification forcée..."
                    
                    // FORCER l'authentification SMTP
                    emailext (
                        to: 'amina1daghari@gmail.com',
                        subject: "TEST AUTH FORCÉE - ${env.BUILD_NUMBER}",
                        body: "Test avec authentification SMTP forcée",
                        mimeType: 'text/plain',
                        // Paramètres critiques
                        smtpAuth: 'true',
                        smtpStartTls: 'true',
                        credentialsId: ''  // Laissez vide pour utiliser les credentials système
                    )
                    
                    echo "✅ Email avec auth forcée envoyé"
                }
            }
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Test Email Ultime') {
            steps {
                script {
                    echo "🔥 Test ULTIME avec toutes les options..."
                    
                    // Définir TOUTES les propriétés SMTP
                    withEnv([
                        'MAIL_SMTP_AUTH=true',
                        'MAIL_SMTP_STARTTLS_ENABLE=true', 
                        'MAIL_SMTP_HOST=smtp.gmail.com',
                        'MAIL_SMTP_PORT=587',
                        'MAIL_SMTP_USER=amina1daghari@gmail.com',
                        'MAIL_SMTP_PASS=drgb csjs hjbn dedj'  // ⚠️ REMPLACEZ
                    ]) {
                        emailext (
                            to: 'amina1daghari@gmail.com',
                            subject: "TEST ULTIME - ${env.BUILD_NUMBER}",
                            body: "Test ULTIME avec toutes les propriétés définies",
                            mimeType: 'text/plain',
                            
                            // Forcer tous les paramètres
                            smtpHost: 'smtp.gmail.com',
                            smtpPort: '587',
                            smtpAuth: 'true',
                            smtpStartTls: 'true',
                            authentication: 'amina1daghari@gmail.com',
                            password: 'drgb csjs hjbn dedj'  // ⚠️ REMPLACEZ
                        )
                    }
                    
                    echo "✅ Test ultime terminé"
                }
            }
        }
    }
}

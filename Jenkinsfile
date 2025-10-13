pipeline {
    agent any

    stages {
        stage('Test Email Simple') {
            steps {
                script {
                    echo "🔍 Test email avec configuration globale..."
                    
                    // Version la plus simple
                    emailext (
                        to: 'amina1daghari@gmail.com',
                        subject: "TEST SIMPLE PIPELINE - ${env.BUILD_NUMBER}",
                        body: "Ceci est un test depuis le pipeline avec config globale",
                        mimeType: 'text/plain'
                    )
                    
                    echo "✅ Email envoyé via configuration globale"
                }
            }
        }
    }
}

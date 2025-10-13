post {
    success {
        script {
            // Test de debug
            echo "🚨 TEST EMAIL - Envoi à: amina1daghari@gmail.com"
            
            // Version simple de test
            mail(
                to: "amina1daghari@gmail.com",
                subject: "TEST Jenkins - ${env.JOB_NAME}",
                body: "Ceci est un test simple. Build: ${env.BUILD_URL}"
            )
            
            echo "📧 Email simple envoyé"
            
            // Attendre un peu
            sleep 30
            
            echo "⏳ Vérifiez vos emails maintenant..."
        }
    }
}

pipeline {
    agent any
    
    stages {
        stage('Test Email Simple') {
            steps {
                echo "🎯 Test d'envoi d'email simple..."
            }
        }
    }
    
    post {
        always {
            // Test avec la méthode mail() simple
            mail(
                to: 'amina1daghari@gmail.com',
                subject: "TEST SIMPLE Jenkins - ${env.JOB_NAME}",
                body: """
                Ceci est un test SIMPLE.
                
                Build: ${env.JOB_NAME} #${env.BUILD_NUMBER}
                URL: ${env.BUILD_URL}
                
                Si vous voyez ceci, ça fonctionne !
                """
            )
            
            echo "📧 Email SIMPLE envoyé à amina1daghari@gmail.com"
        }
    }
}

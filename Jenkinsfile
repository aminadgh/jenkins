pipeline {
    agent any
    tools {
        jdk 'jdk17'
        maven 'Maven 3.9.0'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/aminadgh/jenkins.git'
            }
        }

        stage('Test Email Simple') {
            steps {
                script {
                    echo "🔍 Test d'envoi d'email en cours..."
                    
                    // Test 1: Email simple
                    try {
                        mail(
                            to: 'amina1daghari@gmail.com',
                            subject: "TEST SIMPLE - ${env.JOB_NAME}",
                            body: "Ceci est un test SIMPLE à ${new Date()}"
                        )
                        echo "✅ Email simple envoyé"
                    } catch (Exception e) {
                        echo "❌ Erreur email simple: ${e.message}"
                    }
                    
                    // Test 2: Email étendu
                    try {
                        emailext (
                            to: 'amina1daghari@gmail.com',
                            subject: "TEST EXTENDED - ${env.JOB_NAME}",
                            body: "Ceci est un test EXTENDU à ${new Date()}",
                            mimeType: "text/plain"
                        )
                        echo "✅ Email étendu envoyé"
                    } catch (Exception e) {
                        echo "❌ Erreur email étendu: ${e.message}"
                    }
                }
            }
        }
    }

    post {
        always {
            echo "📧 Résultat final: ${currentBuild.currentResult}"
        }
    }
}

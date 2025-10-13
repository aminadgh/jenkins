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

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test et Rapports') {
            steps {
                sh '''
                mvn test surefire-report:report
                mkdir -p target/site
                '''
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Archivage') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                junit 'target/surefire-reports/*.xml'
            }
        }
    }

    post {
        always {
            echo "✅ Build ${currentBuild.result} - Détails: ${env.BUILD_URL}"
            
            // Publier le rapport de tests
            publishHTML(target: [
                allowMissing: true,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'target/site',
                reportFiles: 'surefire-report.html',
                reportName: 'Rapport de Tests'
            ])
        }
        
        success {
            echo "📧 Envoi de l'email de succès..."
            emailext (
                subject: "✅ SUCCÈS - Build ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                <h2 style="color: #28a745;">Build Réussi ! 🎉</h2>
                
                <div style="background: #f8f9fa; padding: 15px; border-radius: 5px;">
                    <p><strong>📋 Projet:</strong> ${env.JOB_NAME}</p>
                    <p><strong>🔢 Build:</strong> #${env.BUILD_NUMBER}</p>
                    <p><strong>📊 Statut:</strong> <span style="color: #28a745;">SUCCÈS ✅</span></p>
                    <p><strong>⏱️ Durée:</strong> ${currentBuild.durationString}</p>
                    <p><strong>📅 Date:</strong> ${new Date().format('dd/MM/yyyy à HH:mm:ss')}</p>
                </div>
                
                <p><strong>🔗 Détails:</strong> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                
                <hr style="margin: 20px 0;">
                <p style="color: #6c757d;">Cordialement,<br>Votre Pipeline Jenkins CI/CD</p>
                """,
                to: "amina1daghari@gmail.com",
                replyTo: "amina1daghari@gmail.com",
                mimeType: "text/html"
            )
        }
        
        failure {
            echo "📧 Envoi de l'email d'échec..."
            emailext (
                subject: "❌ ÉCHEC - Build ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                <h2 style="color: #dc3545;">Build Échoué ! ⚠️</h2>
                
                <div style="background: #f8f9fa; padding: 15px; border-radius: 5px;">
                    <p><strong>📋 Projet:</strong> ${env.JOB_NAME}</p>
                    <p><strong>🔢 Build:</strong> #${env.BUILD_NUMBER}</p>
                    <p><strong>📊 Statut:</strong> <span style="color: #dc3545;">ÉCHEC ❌</span></p>
                    <p><strong>⏱️ Durée:</strong> ${currentBuild.durationString}</p>
                    <p><strong>📅 Date:</strong> ${new Date().format('dd/MM/yyyy à HH:mm:ss')}</p>
                </div>
                
                <p><strong>🔗 Détails:</strong> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                
                <div style="background: #fff3cd; padding: 10px; border-radius: 5px; border-left: 4px solid #ffc107;">
                    <strong>💡 Cause probable:</strong> Vérifier les tests ou la compilation
                </div>
                
                <hr style="margin: 20px 0;">
                <p style="color: #6c757d;">Action requise !<br>Votre Pipeline Jenkins CI/CD</p>
                """,
                to: "amina1daghari@gmail.com",
                replyTo: "amina1daghari@gmail.com",
                mimeType: "text/html"
            )
        }
        
        unstable {
            echo "📧 Envoi de l'email instable..."
            emailext (
                subject: "⚠️ INSTABLE - Build ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                <h2 style="color: #ffc107;">Build Instable ⚠️</h2>
                
                <div style="background: #f8f9fa; padding: 15px; border-radius: 5px;">
                    <p><strong>📋 Projet:</strong> ${env.JOB_NAME}</p>
                    <p><strong>🔢 Build:</strong> #${env.BUILD_NUMBER}</p>
                    <p><strong>📊 Statut:</strong> <span style="color: #ffc107;">INSTABLE ⚠️</span></p>
                    <p><strong>⏱️ Durée:</strong> ${currentBuild.durationString}</p>
                    <p><strong>📅 Date:</strong> ${new Date().format('dd/MM/yyyy à HH:mm:ss')}</p>
                </div>
                
                <p><strong>🔗 Détails:</strong> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                
                <div style="background: #e2f0ff; padding: 10px; border-radius: 5px; border-left: 4px solid #0d6efd;">
                    <strong>ℹ️ Cause:</strong> Tests échoués mais compilation OK
                </div>
                
                <hr style="margin: 20px 0;">
                <p style="color: #6c757d;">Vérification nécessaire.<br>Votre Pipeline Jenkins CI/CD</p>
                """,
                to: "amina1daghari@gmail.com",
                replyTo: "amina1daghari@gmail.com",
                mimeType: "text/html"
            )
        }
    }
}

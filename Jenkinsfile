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

        stage('Test Email Avant Build') {
            steps {
                script {
                    echo "🔍 TEST EMAIL AVANT BUILD..."
                    try {
                        emailext (
                            subject: "TEST AVANT BUILD - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                            body: """
                            <h2>Test Email AVANT le build</h2>
                            <p>Si vous recevez ceci, les emails fonctionnent au début du pipeline.</p>
                            <p><strong>Time:</strong> ${new Date().format('dd/MM/yyyy HH:mm:ss')}</p>
                            """,
                            to: "amina1daghari@gmail.com",
                            mimeType: "text/html"
                        )
                        echo "✅ Email de test envoyé AVANT build"
                    } catch (Exception e) {
                        echo "❌ Erreur email avant build: ${e.message}"
                    }
                }
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
            script {
                try {
                    emailext (
                        subject: "✅ SUCCÈS - Build ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: """
                        <h2>Build Réussi ! 🎉</h2>
                        <p><strong>Projet:</strong> ${env.JOB_NAME}</p>
                        <p><strong>Build:</strong> #${env.BUILD_NUMBER}</p>
                        <p><strong>Statut:</strong> SUCCÈS ✅</p>
                        <p><strong>Durée:</strong> ${currentBuild.durationString}</p>
                        <p><strong>Détails:</strong> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                        <p><strong>Date:</strong> ${new Date().format('dd/MM/yyyy HH:mm:ss')}</p>
                        <br/>
                        <p>Cordialement,<br/>Jenkins CI/CD</p>
                        """,
                        to: "amina1daghari@gmail.com",
                        replyTo: "amina1daghari@gmail.com",
                        mimeType: "text/html"
                    )
                    echo "✅ Email de succès envoyé avec succès"
                } catch (Exception e) {
                    echo "❌ ERREUR email succès: ${e.message}"
                }
            }
        }
    }
}

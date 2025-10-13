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
            post {
                always {
                    publishHTML(target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'target/site',
                        reportFiles: 'surefire-report.html',
                        reportName: 'Rapport de Tests'
                    ])
                }
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
        }
        success {
            emailext (
                subject: "✅ SUCCÈS - Build ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                <h2>Build Réussi !</h2>
                <p><strong>Projet:</strong> ${env.JOB_NAME}</p>
                <p><strong>Build:</strong> #${env.BUILD_NUMBER}</p>
                <p><strong>Statut:</strong> SUCCÈS ✅</p>
                <p><strong>Durée:</strong> ${currentBuild.durationString}</p>
                <p><strong>Détails:</strong> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                <p><strong>Commit:</strong> ${env.GIT_COMMIT ?: 'N/A'}</p>
                <br/>
                <p>Cordialement,<br/>Jenkins CI/CD</p>
                """,
                to: "dev-team@company.com, admin@company.com",
                replyTo: "jenkins@company.com",
                mimeType: "text/html"
            )
        }
        failure {
            emailext (
                subject: "❌ ÉCHEC - Build ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                <h2>Build Échoué !</h2>
                <p><strong>Projet:</strong> ${env.JOB_NAME}</p>
                <p><strong>Build:</strong> #${env.BUILD_NUMBER}</p>
                <p><strong>Statut:</strong> ÉCHEC ❌</p>
                <p><strong>Durée:</strong> ${currentBuild.durationString}</p>
                <p><strong>Détails:</strong> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                <p><strong>Commit:</strong> ${env.GIT_COMMIT ?: 'N/A'}</p>
                <p><strong>Cause probable:</strong> Vérifier les tests ou la compilation</p>
                <br/>
                <p>Action requise !</p>
                <p>Cordialement,<br/>Jenkins CI/CD</p>
                """,
                to: "dev-team@company.com, admin@company.com",
                replyTo: "jenkins@company.com",
                mimeType: "text/html"
            )
        }
        unstable {
            emailext (
                subject: "⚠️ INSTABLE - Build ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                <h2>Build Instable</h2>
                <p><strong>Projet:</strong> ${env.JOB_NAME}</p>
                <p><strong>Build:</strong> #${env.BUILD_NUMBER}</p>
                <p><strong>Statut:</strong> INSTABLE ⚠️</p>
                <p><strong>Durée:</strong> ${currentBuild.durationString}</p>
                <p><strong>Détails:</strong> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                <p><strong>Cause:</strong> Tests échoués mais compilation OK</p>
                <br/>
                <p>Vérification nécessaire.</p>
                <p>Cordialement,<br/>Jenkins CI/CD</p>
                """,
                to: "dev-team@company.com",
                replyTo: "jenkins@company.com",
                mimeType: "text/html"
            )
        }
    }
}

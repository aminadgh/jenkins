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

        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    // Générer le rapport de tests même si les tests échouent
                    sh 'mvn surefire-report:report'
                    // S'assurer que le répertoire site existe
                    sh 'mkdir -p target/site'
                    publishHTML(target: [
                        allowMissing: true,  // ✅ Changé à true pour éviter l'échec
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

        stage('Analyse Qualité') {
            steps {
                // Vérification du code avec Checkstyle
                sh 'mvn checkstyle:checkstyle'
                // Générer le rapport HTML Checkstyle
                sh 'mvn checkstyle:checkstyle-aggregate'
            }
            post {
                always {
                    publishHTML(target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'target/site',
                        reportFiles: 'checkstyle.html',
                        reportName: 'Rapport Checkstyle'
                    ])
                }
            }
        }

        stage('Archivage') {
            steps {
                // Archiver le JAR généré
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                
                // Archiver les résultats des tests pour JUnit
                junit 'target/surefire-reports/*.xml'
            }
        }
    }

    post {
        always {
            // Nettoyage ou actions finales
            echo "Build ${currentBuild.result} - Voir les détails: ${env.BUILD_URL}"
        }
        success {
            echo '✅ Pipeline exécuté avec succès !'
            // Notification optionnelle
            // emailext subject: "SUCCÈS Build ${env.JOB_NAME}", body: "Build réussi: ${env.BUILD_URL}"
        }
        failure {
            echo '❌ Échec du pipeline !'
            // Notification optionnelle
            // emailext subject: "ÉCHEC Build ${env.JOB_NAME}", body: "Build échoué: ${env.BUILD_URL}"
        }
        unstable {
            echo '⚠️ Build instable (tests échoués mais compilation OK)'
        }
    }
}

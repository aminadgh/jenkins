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
                    script {
                        // Générer le rapport de tests
                        sh 'mvn surefire-report:report || echo "Rapport surefire échoué mais on continue"'
                        
                        // Publier le rapport si disponible
                        if (fileExists('target/site/surefire-report.html')) {
                            publishHTML(target: [
                                allowMissing: false,
                                alwaysLinkToLastBuild: true,
                                keepAll: true,
                                reportDir: 'target/site',
                                reportFiles: 'surefire-report.html',
                                reportName: 'Rapport de Tests'
                            ])
                        } else {
                            echo "⚠️ Rapport de tests non généré"
                        }
                    }
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
                script {
                    // Checkstyle (optionnel - ne pas faire échouer le build)
                    sh 'mvn checkstyle:checkstyle || echo "Checkstyle a échoué mais on continue"'
                    sh 'mvn checkstyle:checkstyle-aggregate || echo "Rapport Checkstyle non généré"'
                    
                    if (fileExists('target/site/checkstyle.html')) {
                        publishHTML(target: [
                            allowMissing: false,
                            alwaysLinkToLastBuild: true,
                            keepAll: true,
                            reportDir: 'target/site',
                            reportFiles: 'checkstyle.html',
                            reportName: 'Rapport Checkstyle'
                        ])
                    }
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
            echo "Build ${currentBuild.result} - Voir les détails: ${env.BUILD_URL}"
        }
        success {
            echo '✅ Pipeline exécuté avec succès !'
        }
        failure {
            echo '❌ Échec du pipeline !'
        }
        unstable {
            echo '⚠️ Build instable (tests échoués mais compilation OK)'
        }
    }
}

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
        stage('Docker Build') {
    steps {
        script {
            sh 'docker build -t student-management:latest .'
        }
    }
}
        stage('Docker Compose Up') {
    steps {
        script {
            // Stop et prune les anciens containers/images si besoin
            sh 'docker compose down || echo "No containers to stop"'
            sh 'docker compose up -d --build'
        }
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
        stage('Analyse SonarQube') {
    environment {
        SONAR_HOST_URL = 'https://sonarcloud.io'
        SONAR_LOGIN = credentials('cd4d6c8a26f144ccf60974ad0792800d1cb46f5f')
    }
    steps {
        sh 'mvn sonar:sonar -Dsonar.projectKey=aminadghjenkins -Dsonar.organization=aminadgh -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONAR_LOGIN'
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

pipeline {
    agent any

    tools {
        jdk 'jdk17'      // ou le nom que tu as configuré dans Jenkins
        maven 'Maven 3.9.0'   // pareil : vérifie le nom exact dans Manage Jenkins > Tools
    }

    stages {
        stage('Checkout') {
            steps {
                // récupère le code depuis ton dépôt GitHub
                git branch: 'main', url: 'https://github.com/aminadgh/jenkins.git'
            }
        }

        stage('Build') {
            steps {
                // compile le projet
                sh 'mvn clean compile'
            }
        }
    }

    post {
        success {
            echo '✅ Compilation réussie !'
        }
        failure {
            echo '❌ Échec de la compilation !'
        }
    }
}


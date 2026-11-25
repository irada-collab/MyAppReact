// Jenkinsfile du TP4 : pipeline pour l'application MERN
pipeline {

    // Jenkins utilise n'importe quel agent disponible
    agent any

    stages {

        // Stage 1 : récupérer le code depuis GitHub
        stage('Clonage') {
            steps {
                git branch: 'main', url: 'https://github.com/irada-collab/MyAppReact.git'
            }
        }

        // Stage 2 : arrêter les anciens conteneurs (évite les conflits de noms)
        stage('Nettoyage') {
            steps {
                bat 'docker compose down'
            }
        }

        // Stage 3 : construire les images Docker
        stage('Build') {
            steps {
                bat 'docker compose build'
            }
        }

        // Stage 4 : lancer l'application (le stage de lancement)
        stage('Lancement') {
            steps {
                bat 'docker compose up -d'
            }
        }
    }

    // Messages affichés à la fin du pipeline
    post {
        success {
            echo 'Application lancée sur http://localhost:3000'
        }
        failure {
            echo 'Le pipeline a échoué, regarde les logs'
        }
    }
}
pipeline {
    agent any

    tools {
        maven 'M3'  // Assurez-vous que Maven est configuré dans Jenkins
        dockerTool 'Docker' // Assurez-vous que Docker est configuré dans Jenkins
    }

    environment {
        SONARQUBE_URL = 'http://localhost:9000' // Remplacez par l'URL de votre serveur SonarQube
        SONARQUBE_TOKEN = credentials('sonar-token') // Utilisez un token SonarQube configuré dans Jenkins
    }

    stages {
        // Étape 1: Cloner le projet depuis GitHub
        stage('Clone GitHub Repository') {
            steps {
                git 'https://github.com/ton-utilisateur/ton-repository.git'
            }
        }

        // Étape 2: Compiler et builder le projet avec Maven
        stage('Build with Maven') {
            steps {
                script {
                    // Exécuter Maven (clean et install)
                    sh 'mvn clean install'
                }
            }
        }

        // Étape 3: Analyser la qualité du code avec SonarQube
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Exécuter l'analyse SonarQube
                    sh 'mvn sonar:sonar -Dsonar.projectKey=your-project-key -Dsonar.host.url=${SONARQUBE_URL} -Dsonar.login=${SONARQUBE_TOKEN}'
                }
            }
        }

        // Étape 4: Construire une image Docker
        stage('Build Docker Image') {
            steps {
                script {
                    // Construire l'image Docker avec le Dockerfile
                    sh 'docker build -t ton-image-name .'
                }
            }
        }
    }

    post {
        // Actions postérieures au pipeline
        success {
            // Archiver les artefacts (fichiers JAR par exemple)
            archiveArtifacts 'target/*.jar'
        }
        failure {
            echo 'Le pipeline a échoué'
        }
    }
}


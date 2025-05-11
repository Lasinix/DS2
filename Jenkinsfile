pipeline {
    agent any

    tools {
        maven 'M3'
        dockerTool 'Docker'
    }

    environment {
        SONARQUBE_URL = 'http://localhost:9000'
        SONARQUBE_TOKEN = credentials('sonar-token')  // Doit être configuré dans Jenkins
    }

    stages {
        stage('Clone GitHub Repository') {
            steps {
                git 'https://github.com/Lasinix/DS2.git'
            }
        }

        stage('Build with Maven') {
            steps {
                bat 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                bat "mvn sonar:sonar -Dsonar.projectKey=DS2 -Dsonar.host.url=${env.SONARQUBE_URL} -Dsonar.login=${env.SONARQUBE_TOKEN}"
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t ds2-image .'
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
        failure {
            echo 'Le pipeline a échoué'
        }
    }
}


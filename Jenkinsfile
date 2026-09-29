pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/ranimchaffar/Timesheet-DevOps.git'
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Build & Run avec Docker Compose') {
            steps {
                sh 'docker compose down || true'
                sh 'docker compose up -d --build'
            }
        }
        stage('Vérifier les conteneurs') {
            steps {
                sh 'docker compose ps'
            }
        }
    }
}

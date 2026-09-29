pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/ranimchaffar/Timesheet-DevOps.git'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Build Jar') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
        stage('Build & Run avec Docker Compose') {
            steps {
                sh 'docker compose down || true'
                sh 'docker rm -f mysql-db timesheet-backend || true'
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

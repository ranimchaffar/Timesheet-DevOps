pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = 'chaffarranim/timesheet-devops'
    }
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
        stage('Test dynamique') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        stage('Test statique') {
            steps {
                sh 'mvn org.apache.maven.plugins:maven-checkstyle-plugin:3.3.0:checkstyle'
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }
        stage('Docker Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Docker Push') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker compose down || true'
                sh 'docker rm -f mysql-db timesheet-backend || true'
                sh 'docker compose pull'
                sh 'docker compose up -d'
            }
        }
        stage('Vérifier les conteneurs') {
            steps {
                sh 'docker compose ps'
            }
        }
    }
}

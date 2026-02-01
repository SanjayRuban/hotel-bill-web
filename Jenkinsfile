pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build JAR (Maven via Docker)') {
            steps {
                sh '''
                docker run --rm \
                  -v "$PWD":/app \
                  -v "$HOME/.m2":/root/.m2 \
                  -w /app \
                  maven:3.9.6-eclipse-temurin-17 \
                  mvn clean package
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t hotel-bill-web .'
            }
        }

        stage('Run Application') {
            steps {
                sh '''
                docker stop hotel-bill || true
                docker rm hotel-bill || true
                docker run -d -p 9090:8080 --name hotel-bill hotel-bill-web
                '''
            }
        }
    }
}

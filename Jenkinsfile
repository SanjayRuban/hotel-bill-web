pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/SanjayRuban/hotel-bill-web.git'
            }
        }

        stage('Build with Maven (Docker)') {
            agent {
                docker {
                    image 'maven:3.9.6-eclipse-temurin-17'
                    args '-v /root/.m2:/root/.m2'
                }
            }
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t hotel-bill-web .'
            }
        }

        stage('Run Docker Container') {
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

pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/SanjayRuban/hotel-bill-web.git'
            }
        }

        stage('Build with Maven') {
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

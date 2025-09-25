pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev/actions', url: 'https://github.com/kurww/frontend-test.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t frontend-test:latest .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                    docker rm -f nextjs-chatbot || true
                    docker run -d --name frontend-test -p 3000:80 frontend-test:latest
                '''
            }
        }
    }
}

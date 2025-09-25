pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/kurww/nextjs-ai-chatbot.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t nextjs-ai-chatbot:latest .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                    docker rm -f nextjs-chatbot || true
                    docker run -d --name nextjs-chatbot -p 3000:3000 nextjs-ai-chatbot:latest
                '''
            }
        }
    }
}

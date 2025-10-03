pipeline {
    agent any

    environment {
        DOCKER_IMAGE   = "frontend-test"
        DOCKERHUB_REPO = "adkurnwn/frontend-test"
        BUILD_TAG      = "dev-actions-${env.GIT_COMMIT}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev/actions', url: 'https://github.com/kurww/frontend-test.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t $DOCKER_IMAGE:$BUILD_TAG .
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        
                        docker tag $DOCKER_IMAGE:$BUILD_TAG $DOCKERHUB_REPO:$BUILD_TAG

                        docker push $DOCKERHUB_REPO:$BUILD_TAG
                    '''
                }
            }
        }
    }

    post {
        always {
            script {
                withCredentials([
                    string(credentialsId: 'telegram-bot-id', variable: 'TELEGRAM_BOT_ID'),
                    string(credentialsId: 'telegram-chat-id', variable: 'TELEGRAM_CHAT_ID'),
                    string(credentialsId: 'telegram-topic-id', variable: 'TELEGRAM_TOPIC_ID')
                ]) {
                    def statusMessage
                    def emoji

                    if (currentBuild.currentResult == 'SUCCESS') {
                        statusMessage = "BERHASIL"
                        emoji = "✅"
                    } else {
                        statusMessage = "GAGAL"
                        emoji = "❌"
                    }

                    def message = """${emoji} Build Notification
                                    --------------------------------------
                                    Project: ${env.JOB_NAME}
                                    Branch: ${env.BRANCH_NAME}
                                    SHA: ${env.GIT_COMMIT}
                                    Build: #${env.BUILD_NUMBER}
                                    Status: *${statusMessage}*
                                    --------------------------------------
                                    Check build log: ${env.BUILD_URL}"""

                    sh """
                        curl -s -X POST https://api.telegram.org/bot\${TELEGRAM_BOT_ID}/sendMessage \\
                        -d chat_id=\${TELEGRAM_CHAT_ID} \\
                        -d message_thread_id=\${TELEGRAM_TOPIC_ID} \\
                        -d parse_mode=Markdown \\
                        --data-urlencode "text=${message}"
                    """
                }
            }
        }
    }
}
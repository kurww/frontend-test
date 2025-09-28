pipeline {
    agent any

    environment {
        DOCKER_IMAGE   = "frontend-test"
        DOCKERHUB_REPO = "adkurnwn/frontend-test"
        BUILD_TAG      = "dev-actions-jenkins-${env.BUILD_NUMBER}"  // unique per Jenkins build
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
                    docker build -t $DOCKER_IMAGE:latest -t $DOCKER_IMAGE:$BUILD_TAG .
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
}

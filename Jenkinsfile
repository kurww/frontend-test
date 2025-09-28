pipeline {
    agent any

    environment {
        DOCKER_IMAGE   = "frontend-test"
        DOCKERHUB_REPO = "adkurnwn/frontend-test"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'staging', url: 'https://github.com/kurww/frontend-test.git'
                
                script {
                    env.GIT_BRANCH = sh(
                        script: "git rev-parse --abbrev-ref HEAD",
                        returnStdout: true
                    ).trim()

                    //SHA
                    env.GIT_COMMIT = sh(
                        script: "git rev-parse HEAD",
                        returnStdout: true
                    ).trim()

                    env.IMAGE_TAG = "${env.GIT_BRANCH}-${env.GIT_COMMIT}"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build \
                        -t $DOCKER_IMAGE:$IMAGE_TAG .
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        
                        docker tag $DOCKER_IMAGE:$IMAGE_TAG $DOCKERHUB_REPO:$IMAGE_TAG
                        docker push $DOCKERHUB_REPO:$IMAGE_TAG
                    '''
                }
            }
        }
    }
}

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
                
                // git sha
                script {
                    env.GIT_COMMIT = sh(
                        script: "git rev-parse --short HEAD",
                        returnStdout: true
                    ).trim()
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build \
                        -t $DOCKER_IMAGE:$GIT_COMMIT .
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        
                        docker tag $DOCKER_IMAGE:$GIT_COMMIT $DOCKERHUB_REPO:$GIT_COMMIT
                        docker push $DOCKERHUB_REPO:$GIT_COMMIT
                        
                    '''
                }
            }
        }
    }
}

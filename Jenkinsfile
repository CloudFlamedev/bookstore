pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'utkrist'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }

        stage('Build book-service') {
            steps {
                echo 'Building book-service Docker image...'
                sh '''
                    cd book-service
                    docker build -t ${DOCKERHUB_USER}/book-service:${IMAGE_TAG} .
                '''
            }
        }

        stage('Build order-service') {
            steps {
                echo 'Building order-service Docker image...'
                sh '''
                    cd order-service
                    docker build -t ${DOCKERHUB_USER}/order-service:${IMAGE_TAG} .
                '''
            }
        }

        stage('Build frontend') {
            steps {
                echo 'Building frontend Docker image...'
                sh '''
                    cd frontend
                    docker build -t ${DOCKERHUB_USER}/bookstore-frontend:${IMAGE_TAG} .
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing images to Docker Hub...'
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push ${DOCKERHUB_USER}/book-service:${IMAGE_TAG}
                        docker push ${DOCKERHUB_USER}/order-service:${IMAGE_TAG}
                        docker push ${DOCKERHUB_USER}/bookstore-frontend:${IMAGE_TAG}
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Deploying to Kubernetes...'
                sh 'kubectl apply -f k8s/'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

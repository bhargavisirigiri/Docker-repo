pipeline {
    agent any

    environment {
        IMAGE_NAME = 'bhargavisirigiri/mypythonapp'  // change to your Docker Hub repo name
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred-id')
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Pulling source code from GitHub...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                echo 'Logging in to Docker Hub...'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'Pushing image to Docker Hub...'
                sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
                sh 'docker tag $IMAGE_NAME:$BUILD_NUMBER $IMAGE_NAME:latest'
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Cleanup') {
            steps {
                echo 'Cleaning up local Docker images...'
                sh 'docker image prune -f'
            }
        }
    }

    post {
        success {
            echo '✅ Build and Push completed successfully!'
        }
        failure {
            echo '❌ Build or Push failed!'
        }
    }
}

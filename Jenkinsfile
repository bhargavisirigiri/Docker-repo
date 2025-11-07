pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = "bhargavi8890/docker-repo"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    echo "🚧 Building Docker image..."
                    sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    echo "🔑 Logging in to Docker Hub..."
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    echo "📤 Pushing Docker image..."
                    sh 'docker push ${IMAGE_NAME}:${BUILD_NUMBER}'
                }
            }
        }
    }

    post {
        success {
            echo "✅ Image pushed successfully!"
        }
        failure {
            echo "❌ Build or Push failed!"
        }
    }
}

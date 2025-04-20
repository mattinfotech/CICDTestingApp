pipeline {
    agent any

    environment {
        IMAGE_NAME = 'fluent-blazor-app'
        DOCKER_IMAGE = 'fluent-blazor-app:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'docker build --no-cache -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Test') {
            steps {
                // Optional: only works if Jenkins node has .NET SDK installed
                // Or use a test container with SDK
                script {
                    echo 'Skipping tests for now...'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker stop fluent-blazor-app || true'
                    sh 'docker rm fluent-blazor-app || true'
                    sh 'docker run -d -p 5000:8080 --name fluent-blazor-app $DOCKER_IMAGE'
                    sh 'docker exec nginx-proxy nginx -s reload || true'
                }
            }
        }
    }

    post {
        always {
            // Optional cleanup
            sh 'docker system prune -f'
        }
    }
}

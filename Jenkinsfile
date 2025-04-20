pipeline {
    agent any

    environment {
        IMAGE_NAME = 'fluent-blazor-app'
        DOCKER_IMAGE = 'fluent-blazor-app:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository to ensure the code is available in the workspace
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // List files in the workspace to verify that the Dockerfile is in the correct location
                    sh 'ls -l'

                    // Build the Docker image
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run tests within the Docker container
                    sh 'docker run --rm $DOCKER_IMAGE dotnet test'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Stop and remove any existing container with the same name
                    sh 'docker stop fluent-blazor-app || true'
                    sh 'docker rm fluent-blazor-app || true'

                    // Run the Docker container in detached mode
                    sh 'docker run -d -p 5000:8080 --name fluent-blazor-app $DOCKER_IMAGE'

                    // Reload nginx proxy if needed
                    sh 'docker exec nginx-proxy nginx -s reload'
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker system to free space after the build process
            sh 'docker system prune -f'
        }
    }
}

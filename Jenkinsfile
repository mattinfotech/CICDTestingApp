pipeline {
    agent any

    environment {
        IMAGE_NAME = 'fluent-blazor-app'
        DOCKER_IMAGE = 'fluent-blazor-app:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm  // Checkout code from GitHub repository
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build the Docker image for the Blazor app
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Add any unit tests you want to run
                    // Example: dotnet test (for .NET projects)
                    sh 'docker run --rm $DOCKER_IMAGE dotnet test'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Push the image to Docker Hub or your Docker registry
                    // sh 'docker push $DOCKER_IMAGE'
                    // Uncomment and use if you have a registry
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Stop the existing container (if any)
                    sh 'docker stop fluent-blazor-app || true'
                    sh 'docker rm fluent-blazor-app || true'

                    // Run the new container
                    sh 'docker run -d -p 5000:8080 --name fluent-blazor-app $DOCKER_IMAGE'

                    // Restart NGINX (if needed) to reflect changes
                    sh 'docker exec nginx-proxy nginx -s reload'
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker containers and images (optional)
            sh 'docker system prune -f'
        }
    }
}

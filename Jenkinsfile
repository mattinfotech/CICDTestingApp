pipeline {
    agent any

    environment {
        IMAGE_NAME = 'fluent-blazor-app'
        DOCKER_IMAGE = 'fluent-blazor-app:latest'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh 'docker run --rm $DOCKER_IMAGE dotnet test'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker stop fluent-blazor-app || true'
                    sh 'docker rm fluent-blazor-app || true'
                    sh 'docker run -d -p 5000:8080 --name fluent-blazor-app $DOCKER_IMAGE'
                    sh 'docker exec nginx-proxy nginx -s reload'
                }
            }
        }
    }

    post {
        always {
            sh 'docker system prune -f'
        }
    }
}

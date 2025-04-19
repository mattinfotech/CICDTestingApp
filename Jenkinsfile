pipeline {
    agent any

    environment {
        IMAGE_NAME = 'cicdtestingapp'
        CONTAINER_NAME = 'cicdtestingapp-container'
    }

    stages {
        stage('Clone Source') {
            steps {
                git credentialsId: 'github-token', url: 'https://github.com/mattinfotech/CICDTestingApp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${IMAGE_NAME}:latest .'
                }
            }
        }

        stage('Stop Previous Container') {
            steps {
                script {
                    sh '''
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    '''
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh 'docker run -d --name ${CONTAINER_NAME} -p 5000:80 ${IMAGE_NAME}:latest'
                }
            }
        }
    }
}

pipeline {
    agent any
    environment {
        IMAGE_NAME = "Komall6/frontend:latest"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/GoogleCloudPlatform/microservices-demo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} src/frontend"
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'docker-creds', url: '']) {
                    sh "docker push ${IMAGE_NAME}"
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh "helm upgrade --install frontend helm/frontend"
            }
        }
        stage('Rolling Update') {
            steps {
                sh "kubectl rollout restart deployment frontend"
            }
        }
    }
}

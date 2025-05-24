pipeline {
    agent any

    environment {
        IMAGE_NAME = "kishoresandy/kishore:latest"
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/kishoresandy/kishore-node-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
                    script {
                        docker.image("${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }

        stage('Deploy to K8s') {
            steps {
                sh 'kubectl apply -f k8s-deployment.yaml'
            }
        }
    }
}


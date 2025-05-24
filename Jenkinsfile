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
            dockerImage = docker.build("kishoresandy/kishore-node-app")
        }
    }
}

stage('Push to DockerHub') {
    steps {
        withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
            script {
                dockerImage.push("latest")
            }
        }
    }
}

        stage('Deploy to K8s') {
    steps {
        withEnv(["KUBECONFIG=/var/lib/jenkins/.kube/config"]) {
            sh 'kubectl apply -f k8s-deployment.yaml'
        }
    }
}

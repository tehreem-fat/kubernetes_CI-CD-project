pipeline {
    agent any
    environment {
        DOCKER_HUB = '<your-dockerhub-username>'
        IMAGE_NAME = 'ci-cd-app'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/tehreem-fat/kubernetes_CI-CD-project.git'
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build -t $DOCKER_HUB/$IMAGE_NAME:latest .'
            }
        }
        stage('Push Image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-token', variable: 'DOCKER_HUB_TOKEN')]) {
                    sh 'echo $DOCKER_HUB_TOKEN | docker login -u $DOCKER_HUB --password-stdin'
                    sh 'docker push $DOCKER_HUB/$IMAGE_NAME:latest'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}

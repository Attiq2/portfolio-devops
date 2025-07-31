pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-creds') // Jenkins credentials ID
        IMAGE_NAME = "yourdockerhubusername/portfolio"
    }

    stages {
        stage('Clone Code') {
            steps {
                git 'https://github.com/yourusername/portfolio-devops.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push $IMAGE_NAME"
            }
        }

        stage('Deploy to Kubernetes (Optional)') {
            when {
                expression { fileExists('k8s/deployment.yaml') }
            }
            steps {
                sh "kubectl rollout restart deployment portfolio-deployment"
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!'
        }
    }
}


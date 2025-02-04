pipeline {
    agent any
    
    environment {
        DOCKER_HUB_USER = 'your-dockerhub-username'
        DOCKER_HUB_PASSWORD = credentials('docker-hub-credentials')
        FRONTEND_IMAGE = 'your-dockerhub-username/frontend-nginx'
        BACKEND_IMAGE = 'your-dockerhub-username/backend-flask'
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your-username/your-repo.git'
            }
        }
        
        stage('Build and Push Docker Images') {
            steps {
                script {
                    sh 'docker build -t $FRONTEND_IMAGE:latest frontend/'
                    sh 'docker build -t $BACKEND_IMAGE:latest backend/'

                    sh 'echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USER --password-stdin'
                    sh 'docker push $FRONTEND_IMAGE:latest'
                    sh 'docker push $BACKEND_IMAGE:latest'
                }
            }
        }
        
        stage('Trigger Argo CD Deployment') {
            steps {
                sh 'argocd app sync frontend-app'
                sh 'argocd app sync backend-app'
            }
        }
    }
}

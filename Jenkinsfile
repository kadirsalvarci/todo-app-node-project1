pipeline {
    agent { label "master" }
    environment {
        ECR_REGISTRY = "644123363827.dkr.ecr.us-east-1.amazonaws.com"
        APP_REPO_NAME= "clarusway-repo/todo-app"
    }
    stages {
        stage('Build Docker Image') {
            steps {
                sh 'sudo docker build --force-rm -t "$ECR_REGISTRY/$APP_REPO_NAME:latest" .'
                sh 'sudo docker image ls'
            }
        }
        stage('Push Image to ECR Repo') {
            steps {
                sh 'sudo aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin "$ECR_REGISTRY"'
                sh 'sudo docker push "$ECR_REGISTRY/$APP_REPO_NAME:latest"'
            }
        }
    }
    post {
        always {
            echo 'Deleting all local images'
            sh 'sudo docker image prune -af'
        }
    }
}

pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"
        ECR_REGISTRY = "021891571564.dkr.ecr.ap-south-1.amazonaws.com"
        ECR_REPOSITORY = "devops-project"
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/JoyalBBiju/devops-capstone-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-project .'
            }
        }

        stage('Security Scan - Trivy') {
            steps {
                sh 'trivy image --exit-code 1 --severity HIGH,CRITICAL devops-project'
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag devops-project:latest $ECR_REGISTRY/$ECR_REPOSITORY:latest'
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS --password-stdin $ECR_REGISTRY
                '''
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $ECR_REGISTRY/$ECR_REPOSITORY:latest'
            }
        }

    }
}

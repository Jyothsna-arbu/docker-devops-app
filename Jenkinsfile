pipeline {
    agent any
    environment {
        AWS_REGION = 'ap-south-1'
        ECR_REPO = 'docker-devops-app'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Jyothsna-arbu/docker-devops-app.git',
            }
        }

    stage('Docker Build') {
        steps {
            bat 'docker build -t ${ECR_REPO}:${IMAGE_TAG} .'        
        }
    }

    stage('Docker Push to ECR') {
        steps {
            withAWS(credentials: 'aws-credentials', region: 'ap-south-1')   {
                bat '''
                aws ecr get-login-password --region %AWS_REGION% | docker login --username AWS --password-stdin 951588414270.dkr.ecr.ap-south-1.amazonaws.com
                docker tag %ECR_REPO%:%IMAGE_TAG% 951588414270.dkr.ecr.%AWS_REGION%.amazonaws.com/%ECR_REPO%:%IMAGE_TAG%
                docker push 951588414270.dkr.ecr.%AWS_REGION%.amazonaws.com/%ECR__REPO%:%IMAGE_TAG%
                '''
            }      
        }
    }
  }
}
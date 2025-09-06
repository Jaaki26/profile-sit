pipeline {
    agent any
    environment {
        AWS_REGION = "ap-south-1"
        ECR_REPO = "profile-site"
        AWS_ACCOUNT_ID = "123456789012"
        FULL_IMAGE = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}:${BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('SonarQube Scan') {
            steps {
                script { echo "SonarQube scan here" }
            }
        }
        stage('Docker Build & Push') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION |                 docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                docker build -t $FULL_IMAGE .
                docker push $FULL_IMAGE
                '''
            }
        }
        stage('Update Manifests') {
            steps { sh './scripts/update_image_tag.sh $FULL_IMAGE' }
        }
    }
}
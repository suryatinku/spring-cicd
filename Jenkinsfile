pipeline {
environment {
AWS_ACCOUNT_ID="098324025508"
AWS_DEFAULT_REGION="ap-south-1"
IMAGE_REPO_NAME="javademo"
IMAGE_TAG="${BUILD_NUMBER}"
REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
}

    tools { 
        maven 'Maven 3.8.7' 
    }
  agent any
  stages {
    stage('Build') {
      steps{
        script {
          sh 'mvn clean install'
        }
      }
    }
    stage('Load') {
      steps{
        script {
          dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
        }
      }
    }
     stage('login into ecr') {
      steps{
        script {
           sh "sudo aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
          }
        }
      }
 
stage('Push Image') {
      steps{
         script {
             sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:${IMAGE_TAG}"
             sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"

          }
        }
      }    
    
    stage('Deploy to EKS'){
      steps{
          script {
            sh 'kubectl apply -f sample.yaml'
      }
    }
  }
    
  }
}

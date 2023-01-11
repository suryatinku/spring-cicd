pipeline {
  environment {
    registryCredential = "docker"
  }
  agent any
  stages {
    stage(‘Build’) {
      steps{
        script {
          sh 'mvn clean install'
        }
      }
    }
    stage(‘Load’) {
      steps{
        script {
          app = docker.build("achyuth007/simple-spring")
        }
      }
    }
     stage(‘Deploy’) {
      steps{
        script {
          docker.withRegistry( "https://registry.hub.docker.com", registryCredential ) {
           // dockerImage.push()
          app.push("latest")
          }
        }
      }
    }
    stage('Deploy to ACS'){
      steps{
          withCredentials([azureServicePrincipal('dbb6d63b-41ab-4e71-b9ed-32b3be06eeb8')]) {
            sh 'echo "logging in" '
            sh 'az login --service-principal -u c5ceb42a-033d-4dcf-bc2b-b2a7b37bff21 -p xyeBmx1bynF2Z6T+dzCgklfQ+1CuNPI6aY7EdIfE0OI= -t be10e06f-0415-4faf-8faf-d4ccf24c1ede'
            sh 'az account set -s 1e5fc2e8-f4df-4895-9f77-00e140031cb2'
            sh 'az aks get-credentials --resource-group ilink --name gajacluster'
            sh 'kubectl apply -f sample.yaml'
      }
    }
  }
  }
}

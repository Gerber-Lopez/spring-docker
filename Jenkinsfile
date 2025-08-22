pipeline {
  agent any
  environment {
    IMAGE_NAME = "demo-ci-cd"
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build & Test') {
      steps {
        sh 'mvn -B clean package'
      }
    }
    stage('Build Docker Image') {
      steps {
        sh 'sudo docker build -t $IMAGE_NAME .'
      }
    }
    stage('Run Container') {
      steps {
        sh 'sudo docker rm -f $IMAGE_NAME || true'
        sh 'sudo docker run -d --name $IMAGE_NAME -p 8082:8080 $IMAGE_NAME:latest'
      }
    }
  }
  post {
    always {
      junit '**/target/surefire-reports/*.xml'
      archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
    }
  }
}

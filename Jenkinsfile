pipeline {
  agent any
  stages {
    stage('Clone') {
      steps {
        git 'https://github.com/Vennilavan12/trend.git'
      }
    }
    stage('Build Docker') {
      steps {
        sh 'docker build -t srinivasamurthym/trend-app:latest .'
      }
    }
    stage('Push Docker') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
          sh 'echo $PASS | docker login -u $USER --password-stdin'
          sh 'docker push srinivasamurthym/trend-app:latest'
        }
      }
    }
    stage('Deploy to EKS') {
      steps {
        sh 'kubectl apply -f deployment.yaml'
        sh 'kubectl apply -f service.yaml'
      }
    }
  }
}

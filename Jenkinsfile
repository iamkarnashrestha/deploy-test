pipeline {
  agent any
  stages {
    stage('Clone Repository') {
      steps {
        git 'https://github.com/iamkarnashrestha/deploy-test.git'
      }
    }
    stage('Build Docker Image') {
      steps {
        script {
          docker.build("username/node-app:${env.BUILD_ID}")
        }
      }
    }
    stage('Push Docker Image') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
            docker.image("username/node-app:${env.BUILD_ID}").push()
          }
        }
      }
    }
    stage('Deploy to Kubernetes') {
      steps {
        kubernetesDeploy(configs: 'k8s-deployment.yaml', kubeconfigId: 'kubernetes-credentials')
      }
    }
  }
}

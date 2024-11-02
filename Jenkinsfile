pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "karnastha/node-app"
        K8S_DEPLOYMENT_FILE = "k8s-deployment.yaml"
    }
    stages {
        stage('Build Docker Image') {
    steps {
        script {
            sh '/usr/local/bin/docker build -t karnastha/node-app:${env.BUILD_ID} .'
        }
    }
}

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                        docker.image("${DOCKER_IMAGE}:${env.BUILD_ID}").push()
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                kubernetesDeploy(configs: "${K8S_DEPLOYMENT_FILE}", kubeconfigId: 'kubernetes-credentials')
            }
        }
    }
    post {
        always {
            cleanWs() // Clean up workspace after build
        }
    }
}

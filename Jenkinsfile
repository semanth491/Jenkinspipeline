pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    docker.build("nginx_image")
                }
            }
        }
        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        docker.image('nginx_image').push()
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    withCredentials([kubeconfigFile(credentialsId: 'kubernetes-credentials', variable: 'KUBECONFIG')]) {
                        sh 'kubectl apply -f deployment.yaml'
                        sh 'kubectl apply -f service.yaml'
                    }
                }
            }
        }
    }
}

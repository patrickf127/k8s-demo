pipeline {
    agent any

    environment {
        PATH = "/var/jenkins_home/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Validate') {
            steps { sh 'kubectl apply --dry-run=server -f deployment.yaml' }
        }
        stage('Deploy') {
            steps { sh 'kubectl apply -f deployment.yaml' }
        }
        stage('Verify') {
            steps {
                sh 'kubectl rollout status deployment/nginx --timeout=120s'
                sh 'kubectl get pods -l app=nginx -o wide'
            }
        }
    }
}

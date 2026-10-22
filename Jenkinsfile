pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  serviceAccountName: jenkins-deployer
  containers:
    - name: kubectl
      image: alpine/k8s:1.33.13
      command: ["sleep"]
      args: ["99d"]
'''
        }
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Validate') {
            steps {
                container('kubectl') {
                    sh 'kubectl apply --dry-run=server -f deployment.yaml'
                }
            }
        }
        stage('Deploy') {
            steps {
                container('kubectl') {
                    sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
        stage('Verify') {
            steps {
                container('kubectl') {
                    sh 'kubectl rollout status deployment/nginx --timeout=120s'
                    sh 'kubectl get pods -l app=nginx -o wide'
                }
            }
        }
    }
}

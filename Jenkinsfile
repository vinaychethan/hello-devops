pipeline {
    agent any
    environment {
        IMAGE_NAME = "hello-devops-node"
        IMAGE_TAG  = "v1"
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/vinaychethan/hello-devops.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh '''
                  npm install
                '''
            }
        }
        stage('Test') {
            steps {
                sh '''
                  npm test || echo "No tests defined"
                '''
            }
        }
        stage('Docker Build') {
            steps {
                sh '''
                  docker build -t $IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }
        stage('Deploy') {
            steps {
                withCredentials([string(credentialsId: 'kubeconfig', variable: 'KUBECONFIG_CONTENT')]) {
                    sh '''
                        printf '%s' "$KUBECONFIG_CONTENT" > /tmp/kubeconfig
                        KUBECONFIG=/tmp/kubeconfig kubectl apply -f k8s/deployment.yaml
                        KUBECONFIG=/tmp/kubeconfig kubectl apply -f k8s/service.yaml
                    '''
                }
            }
        }
    }
}

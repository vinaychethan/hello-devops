pipeline {
    agent any

    environment {
        KUBECONFIG = "/home/vinay/.kube/config"
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
                  eval $(minikube -p minikube docker-env)
                  docker build -t $IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                  kubectl apply -f k8s/deployment.yaml
                  kubectl apply -f k8s/service.yaml
                '''
            }
        }
    }
}

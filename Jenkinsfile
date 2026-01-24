pipeline {
    agent any   // self-hosted runner

    environment {
        IMAGE_NAME = "hello-devops:1.0"   // Using Minikube Docker environment
    }

    stages {

        stage('Build') {
            steps {
                echo "✅ Build Stage"
                checkout scm
                sh 'node -v'
                sh 'npm -v'
                sh 'npm ci'
                sh 'npm run build || echo "No build step"'
            }
        }

        stage('Test') {
            steps {
                echo "✅ Test Stage"
                sh 'npm test'
            }
        }

        stage('Docker') {
            steps {
                echo "✅ Docker Stage"
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy') {
            steps {
                echo "✅ Deploy Stage"
                sh 'eval $(minikube docker-env)'
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
                sh 'kubectl rollout status deployment/hello-devops'
                sh 'kubectl get pods'
            }
        }

    }
}

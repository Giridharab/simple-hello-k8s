pipeline {
    agent any

    environment {
        IMAGE_NAME = "localhost:5000/hello-app"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Giridharab/simple-hello-k8s.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest app/'
            }
        }

        stage('Push to Local Registry') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                  kubectl apply -f app/deployment.yaml
                  kubectl rollout status deployment/hello-app
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Application deployed successfully!"
            sh "kubectl get svc hello-app-service -o wide"
        }
    }
}

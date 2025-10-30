pipeline {
    agent any

    environment {
        DOCKERHUB_USER = credentials('dockerhub-credentials')  // Jenkins Credential ID
        IMAGE_NAME = "docker.io/${DOCKERHUB_USER}/hello-app"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Giridharab/simple-hello-k8s.git', branch: 'main'
            }
        }

        stage('Docker Login') {
            steps {
                sh 'echo $DOCKERHUB_USER_PSW | docker login -u $DOCKERHUB_USER_USR --password-stdin'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest app/'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                  sed -i "s|image:.*|image: $IMAGE_NAME:latest|g" app/deployment.yaml
                  kubectl apply -f app/deployment.yaml
                  kubectl rollout status deployment/hello-app
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Application deployed successfully from Docker Hub!"
            sh "kubectl get svc hello-app-service -o wide"
        }
    }
}

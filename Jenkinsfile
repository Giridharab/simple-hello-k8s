pipeline {
    agent any

     environment {
        IMAGE_NAME = "giridmur7/hello-app"  // ✅ set your Docker Hub repo name
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'GITHUB_TOKEN', url: 'https://github.com/Giridharab/simple-hello-k8s.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest app/'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'DOCKER_HUB_CREDENTIALS',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $IMAGE_NAME:latest
                    """
                }
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

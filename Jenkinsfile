pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-app"
        DOCKER_HUB_USER = "rohitgovindaraju"
    }
    
        stage('Build Docker Image') {
            steps {
                dir('app') {
                    script {
                        sh "docker build -t $DOCKER_HUB_USER/$IMAGE_NAME:latest ."
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'rohitgovindaraju', passwordVariable: 'Asdfghjkl@1')]) {
                        sh "echo $PASSWORD | docker login -u $USERNAME --password-stdin"
                        sh "docker push $DOCKER_HUB_USER/$IMAGE_NAME:latest"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh "kubectl apply -f k8s/deployment.yaml"
                }
            }
        }
    }
}

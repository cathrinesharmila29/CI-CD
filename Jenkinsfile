pipeline {
    agent any 
    environment {
        DOCKER_IMAGE = "cathrine29/flaskapp"
    }
    stages {
        stage('Build docker image') {
            steps {  
                echo "Building Docker image..."
                sh "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials',
                                                  usernameVariable: 'DOCKERHUB_USERNAME',
                                                  passwordVariable: 'DOCKERHUB_TOKEN')]) {
                    echo "Logging in to Docker Hub..."
                    sh "echo $DOCKERHUB_TOKEN | docker login -u $DOCKERHUB_USERNAME --password-stdin"
                }
            }
        }
        stage('Push image') {
            steps {
                echo "Pushing Docker image to Docker Hub..."
                sh "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
            }
        }
        stage('Deploy to Staging') {
            steps {
                sh "docker pull ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                sh 'docker stop myapp-container || true'
                sh 'docker rm myapp-container || true'
                sh "docker run -d --name myapp-container -p 8000:8000 ${DOCKER_IMAGE}:${BUILD_NUMBER}"
            }
        }
    }
    post {
        always {
            echo "Logging out from Docker Hub..."
            sh 'docker logout'
        }
    }
}

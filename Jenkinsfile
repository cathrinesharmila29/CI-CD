def imageName = "cathrine29/flaskapp".toLowerCase()

stage('Build docker image') {
    steps {
        sh "docker build -t ${imageName}:${BUILD_NUMBER} ."
    }
}

stage('push image') {
    steps {
        sh "docker push ${imageName}:${BUILD_NUMBER}"
    }
}

stage('Deploy to Staging') {
    steps {
        sh "docker pull ${imageName}:${BUILD_NUMBER}"
        sh 'docker stop myapp-container || true'
        sh 'docker rm myapp-container || true'
        sh "docker run -d --name myapp-container -p 8000:8000 ${imageName}:${BUILD_NUMBER}"
    }
}

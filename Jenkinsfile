pipeline {
agent any

environment {
    DOCKER_IMAGE = "rajeshtutta123/choco"
}

stages {

    stage('Clone Repository') {
        steps {
            git branch: 'main', url: 'https://github.com/rajeshtutta/choco.git'
        }
    }

    stage('Build Docker Image') {
        steps {
            sh 'docker build -t $DOCKER_IMAGE:latest .'
        }
    }

    stage('Push Docker Image') {
        steps {
            withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                sh 'echo $PASS | docker login -u $USER --password-stdin'
                sh 'docker push $DOCKER_IMAGE:latest'
            }
        }
    }

    stage('Deploy to Kubernetes') {
        steps {
            sh 'kubectl apply -f k8s-deployment.yml'
            sh 'kubectl apply -f k8s-service.yml'
        }
    }
}

}

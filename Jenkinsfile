pipeline {
    agent any

    environment {
        IMAGE_NAME = "vasanth4747/education-animation"
        TAG = "v1"
        DOCKER_USERNAME = "vasanth4747"
        DOCKER_PASSWORD = "vasanth@47" // Avoid using in production
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/vasanthvk47/education-anie.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${TAG}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Using plain text credentials (not recommended in production)
                    sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    docker.image("${IMAGE_NAME}:${TAG}").push()
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Applying Kubernetes configurations, bypassing validation errors
                sh 'kubectl apply -f k8s/deployment.yaml --validate=false'
                sh 'kubectl apply -f k8s/service.yaml --validate=false'
            }
        }
    }
}

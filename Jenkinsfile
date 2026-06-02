pipeline {
    agent any

    tools {
        maven 'Maven-3'
    }

    environment {
        IMAGE_NAME = "java-cicd-demo"
        CONTAINER_NAME = "java-cicd-container"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/YOUR_USERNAME/java-cicd-demo.git'
            }
        }

        stage('Build Java App') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Remove Old Container') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Deploy New Container') {
            steps {
                sh '''
                docker run -d \
                --name $CONTAINER_NAME \
                $IMAGE_NAME:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed. Check console output.'
        }
    }
}
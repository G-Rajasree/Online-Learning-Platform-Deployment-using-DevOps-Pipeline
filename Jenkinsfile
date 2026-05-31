pipeline {
    agent any

    environment {
        IMAGE_NAME = "grajasree/ecommerce-app"
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/G-Rajasree/Online-Learning-Platform-Deployment-using-DevOps-Pipeline.git'
            }
        }

        stage('Test') {
            steps {
                sh 'echo Running Tests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop ecommerce-app || true
                docker rm ecommerce-app || true

                docker run -d \
                --name ecommerce-app \
                -p 80:80 \
                $IMAGE_NAME:latest
                '''
            }
        }
    }
}

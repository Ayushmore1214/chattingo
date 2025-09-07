pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials') 
        DOCKERHUB_USERNAME = "yourdockerhubusername"
        REPO_NAME = "chattingo"
    }

    stages {
        stage('Git Clone') {
            steps {
                echo 'Cloning the repository...'
                git branch: 'main', url: "https://github.com/Ayushmore1214/chattingo.git"
            }
        }

        stage('Build Images') {
            steps {
                echo 'Building Docker images with sudo...'
                sh 'sudo docker-compose build'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo 'Logging into Docker Hub and pushing images with sudo...'
                withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIALS, passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "sudo docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"

                    sh "sudo docker tag chattingo-frontend:latest ${DOCKERHUB_USERNAME}/${REPO_NAME}-frontend:latest"
                    sh "sudo docker tag chattingo-backend:latest ${DOCKERHUB_USERNAME}/${REPO_NAME}-backend:latest"

                    sh "sudo docker push ${DOCKERHUB_USERNAME}/${REPO_NAME}-frontend:latest"
                    sh "sudo docker push ${DOCKERHUB_USERNAME}/${REPO_NAME}-backend:latest"
                    sh "sudo docker logout"
                }
            }
        }
        
        stage('Deploy to Production') {
            steps {
                echo 'Deploying application to the VPS with sudo...'
                sh 'cp /var/lib/jenkins/.env .'
                sh 'sudo docker-compose pull'
                sh 'sudo docker-compose up -d'
            }
        }
    }
}
pipeline {
    agent any

    environment {
        // The credential ID we set up in the Jenkins UI
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials') 
        DOCKERHUB_USERNAME = "yourdockerhubusername"
        REPO_NAME = "chattingo"
    }

    stages {
        stage('Git Clone') {
            steps {
                echo 'Cloning the repository...'
                git branch: 'main', url: "https://github.com/YOUR_USERNAME/chattingo.git"
            }
        }

        stage('Build Images') {
            steps {
                echo 'Building Docker images...'
                sh 'docker-compose build'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo 'Logging into Docker Hub and pushing images...'
                withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIALS, passwordVariable: 'DOCK-ER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"

                    sh "docker tag chattingo-frontend:latest ${DOCKERHUB_USERNAME}/${REPO_NAME}-frontend:latest"
                    sh "docker tag chattingo-backend:latest ${DOCKERHUB_USERNAME}/${REPO_NAME}-backend:latest"

                    sh "docker push ${DOCKERHUB_USERNAME}/${REPO_NAME}-frontend:latest"
                    sh "docker push ${DOCKERHUB_USERNAME}/${REPO_NAME}-backend:latest"
                    sh "docker logout"
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying application to the VPS...'
                // This stage will pull the new images from Docker Hub and restart the services
                sh 'docker-compose pull'
                sh 'docker-compose up -d'
            }
        }
    }
}
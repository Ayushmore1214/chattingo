pipeline {
    agent any

    // We define the parameters we just created in the Jenkins UI
    parameters {
        string(name: 'DOCKER_USER', defaultValue: '', description: 'Your Docker Hub Username')
        password(name: 'DOCKER_PASS', defaultValue: '', description: 'Your Docker Hub Password or Token')
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
                echo 'Logging into Docker Hub and pushing images directly...'
                // This uses the parameters directly instead of the credential store
                sh "sudo docker login -u ${params.DOCKER_USER} -p ${params.DOCKER_PASS}"

                sh "sudo docker tag chattingo-pipeline_frontend:latest ${params.DOCKER_USER}/chattingo-frontend:latest"
                sh "sudo docker tag chattingo-pipeline_backend:latest ${params.DOCKER_USER}/chattingo-backend:latest"

                sh "sudo docker push ${params.DOCKER_USER}/chattingo-frontend:latest"
                sh "sudo docker push ${params.DOCKER_USER}/chattingo-backend:latest"
                sh "sudo docker logout"
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
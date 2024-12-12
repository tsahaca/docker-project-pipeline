pipeline {
    agent any
    environment{
        DOCKER_USERNAME = 'tsaha'
        FRONTEND_IMAGE = 'frontend'
        BACKEND_IMAGE = 'backend'
        REGISTRY_CREDENTIALS = 'd1a29db5-18ee-4544-954e-236162fccb99'
    }
    
    stages{
        stage('Docker Installs'){
            steps{
                sh 'sudo apt-get update -y && sudo apt-get upgrade -y'
                sh 'sudo apt-get install -y docker-compose'
            }
        }
        
        stage('Clone my-docker-app Repository'){
            steps{
                git branch: 'main', url: "https://github.com/tsahaca/my-docker-app.git"
            }
        }
        
        stage('Build Docker Images with docker-compose'){
            steps{
                script{
                    sh 'docker-compose build'
                    sh 'docker tag docker-project-pipeline_client:latest $DOCKER_USERNAME/$FRONTEND_IMAGE:latest'
                    sh 'docker tag docker-project-pipeline_server:latest $DOCKER_USERNAME/$BACKEND_IMAGE:latest'
                }
            }
        }
        
        stage('Push Docker Images'){
            steps{
                script{
                    withDockerRegistry([credentialsId: REGISTRY_CREDENTIALS, url: 'https://index.docker.io/v1/']){
                        sh 'docker push $DOCKER_USERNAME/$FRONTEND_IMAGE:latest'
                        sh 'docker push $DOCKER_USERNAME/$BACKEND_IMAGE:latest'
                    }
                }
            }
        }
        
        stage('Deploy Docker Containers'){
            steps{
                script {
                    sh 'docker-compose up -d'
                }
            }
        }
    }
    
    post{
        always{
            echo 'Pipeline execution completed'
        }
    }
}

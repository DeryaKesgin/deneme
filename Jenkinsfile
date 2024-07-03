pipeline {
    agent any
    tools {
        maven 'maven'
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // Docker Hub credentials id
        DOCKERHUB_REPO = 'your-dockerhub-username/demo12' // Docker Hub repository
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/DeryaKesgin/jenkinsdeneme']]
                )
                bat 'mvn clean install'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKERHUB_REPO}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKERHUB_CREDENTIALS}") {
                        docker.image("${DOCKERHUB_REPO}:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop and remove any existing container with the same name
                    sh 'docker stop demo-container || true'
                    sh 'docker rm demo-container || true'
                    
                    // Run the new container
                    docker.image("${DOCKERHUB_REPO}:${env.BUILD_NUMBER}").run("-d -p 4444:8080 --name demo-container")
                }
            }
        }
    }
}

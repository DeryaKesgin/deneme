pipeline {
    agent any
    tools {
        maven 'maven'
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


        stage('Build docker image'){
            steps{
                script{
                    docker.build("demo12:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                    docker.image("demo12:${env.BUILD_NUMBER}").run("-d -p 4444:8080 --name demo-container")
                }
            }
  }
}

}

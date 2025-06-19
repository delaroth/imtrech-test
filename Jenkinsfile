pipeline {
    agent any

    environment {
        DOCKER_HUB_USERNAME = credentials('DOCKER_HUB_USERNAME') // Jenkins Credential ID for Docker Hub username
        DOCKER_HUB_TOKEN = credentials('DOCKER_HUB_TOKEN')       // Jenkins Credential ID for Docker Hub token
        IMAGE_NAME = 'my-nginx-app'
    }

    stages {
        stage('Build and Push Docker Image') {
            steps {
                script {
                    def imageTag = new Date().format('yyMMddHHmm')
                    sh "docker build -t ${env.DOCKER_HUB_USERNAME}/${env.IMAGE_NAME}:${imageTag} ."
                    withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB_USERNAME', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                        sh "docker push ${env.DOCKER_HUB_USERNAME}/${env.IMAGE_NAME}:${imageTag}"
                        sh "docker logout" // It's good practice to log out
                    }
                }
            }
        }
    }
}

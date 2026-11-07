pipeline {
    agent any 

    environment {
        IMAGE_NAME = "phanindra61/java-demo"
        TAG = "${BUILD_NUMBER}" 
        FULL_IMAGE = "${IMAGE_NAME}:${TAG}"

    }

    stages {

        stage('Clone'){
            steps {
                git branch: 'main', url: 'https://github.com/phanindra61/simple-java-app.git'
            }
        }
        
        stage('Build application') {
            steps {
                sh 'java app.java'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'DOCKERHUB_ID', 
                    passwordVariable: 'DOCKER_PASS', 
                    usernameVariable: 'DOCKER_USER'
                )]) {

                        sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin' 
                }
            }
        }

        stage('Build docker image') {
            steps {
                sh 'docker build -t ${FULL_IMAGE} .'
            }
        }

        stage('Push to dockerhub') {
            steps{
                sh 'docker push ${FULL_IMAGE}'
            }
        }

        stage('Run container'){
            steps{
                sh 'docker run --rm -it -d -p 80:80 ${FULL_IMAGE} sleep 300' 
            }
        }
    }
}

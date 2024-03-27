pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = "java-app-xxx102144"
        DOCKERHUB_USERNAME = "altunarali"
        SECOND_SERVER_USERNAME = "altunarali"
        SECOND_SERVER_IP = "10.0.2.5"
        SECOND_SERVER_PASSWORD = "debian"
        PATH = "$PATH:/opt/apache-maven-3.9.6/bin"
        DOCKER_HUB_CREDENTIALS_ID = "dckr_pat_ZJi4yNE8yUqcnhQwFWBId5oua7s" // Jenkins credentials'ların id'si
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/MohanBEEEE/Jenkins-pipeline-to-push-DockerImg-to-DockerHub'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Create Docker Image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE_NAME, '.')
                }
            }
        }
        stage('Push Image to Docker Hub') {
            steps {
                script {
                    withDockerRegistry([ credentialsId: DOCKER_HUB_CREDENTIALS_ID, url: "" ]) {
                        bat "docker push ${DOCKER_IMAGE_NAME}:latest"
                    }
                }
            }
        }

        stage('Deploy to Second Linux Server') {
            steps {
                script {
                    sh "sshpass -p ${SECOND_SERVER_PASSWORD} ssh ${SECOND_SERVER_USERNAME}@${SECOND_SERVER_IP} 'docker pull ${DOCKER_IMAGE_NAME}'"
                    sh "sshpass -p ${SECOND_SERVER_PASSWORD} ssh ${SECOND_SERVER_USERNAME}@${SECOND_SERVER_IP} 'docker run -d --name java-app-container -p 8080:8080 ${DOCKER_IMAGE_NAME}'"
                }
            }
        }
    }
}

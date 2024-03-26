pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = "java-app-xxx102144"
        DOCKER_HUB_USERNAME = "altunarali"
        DOCKER_HUB_PASSWORD = "361330258Aa"
        SECOND_SERVER_USERNAME = "altunarali"
        SECOND_SERVER_IP = "10.0.2.5"
        SECOND_SERVER_PASSWORD = "debian"
        PATH = "$PATH:/opt/apache-maven-3.9.6/bin"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'hhttps://github.com/MohanBEEEE/Jenkins-pipeline-to-push-DockerImg-to-DockerHub'
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
        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_HUB_USERNAME, DOCKER_HUB_PASSWORD) {
                        docker.image(DOCKER_IMAGE_NAME).push('latest')
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

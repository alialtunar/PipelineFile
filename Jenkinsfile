pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = "java-app-xxx102144"
        DOCKER_HUB_ACCESS_TOKEN = "dckr_pat_ZJi4yNE8yUqcnhQwFWBId5oua7s" 
        SECOND_SERVER_USERNAME = "altunarali"
        DOCKERHUB_USERNAME = "altunarali"
        SECOND_SERVER_IP = "10.0.2.5"
        SECOND_SERVER_PASSWORD = "debian"
        PATH = "$PATH:/opt/apache-maven-3.9.6/bin"
    }

    stages {
        stage('Initialize') {
            steps {
                script {
                    def dockerHome = tool 'myDocker'
                    env.PATH = "${dockerHome}/bin:${env.PATH}"
                }
            }
        }
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
        stage('Build and Push image') {
            steps {
                withDockerRegistry(credentialsId: 'altunarali', toolName: 'myDocker') {
                    sh "docker build -t altunarali/jenkins1:tag123 ."
                    sh "docker push altunarali/jenkins1:tag123"  
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

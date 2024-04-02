pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = "java-app-xxx102144"
        DOCKER_HUB_ACCESS_TOKEN = "dckr_pat_ZJi4yNE8yUqcnhQwFWBId5oua7s" 
        SECOND_SERVER_USERNAME = "debian"
        DOCKERHUB_USERNAME = "altunarali"
        SECOND_SERVER_IP = "10.0.2.7"
        SECOND_SERVER_PASSWORD = "debian"
        PATH = "$PATH:/opt/apache-maven-3.9.6/bin"



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
      stage('Build and Push image') {
    steps {
        script {
            echo "Build and Push image aşamasına başlanıyor..."
            withDockerRegistry(credentialsId: 'acaracar') {
                echo "Docker Registry'ye bağlanıldı..."
                sh "docker build -t altunarali/jenkins1:tag123 ."
                echo "Docker imajı oluşturuldu..."
                sh "docker push altunarali/jenkins1:tag123"
                echo "Docker imajı Docker Registry'e gönderildi..."
            }
        }
    }
}
        stage('Deploy to Second Linux Server') {
            steps {
                script {
                   
                    
                    sh "sshpass -p ${SECOND_SERVER_PASSWORD} ssh -o StrictHostKeyChecking=no ${SECOND_SERVER_USERNAME}@${SECOND_SERVER_IP} 'docker pull altunarali/jenkins1:tag123'"
                    sh "sshpass -p ${SECOND_SERVER_PASSWORD} ssh -o StrictHostKeyChecking=no ${SECOND_SERVER_USERNAME}@${SECOND_SERVER_IP} 'docker run -d --name java-app-container -p 8080:8080 altunarali/jenkins1:tag123'"
                }
            }
        }
    }
}

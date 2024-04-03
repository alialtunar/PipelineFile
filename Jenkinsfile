pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = credentials("DOCKER_IMAGE_NAME")
        SECOND_SERVER_USERNAME = credentials(" SECOND_SERVER_USERNAME")
        DOCKERHUB_USERNAME =  credentials("DOCKERHUB_USERNAME")
        SECOND_SERVER_IP = credentials("SECOND_SERVER_IP")
        SECOND_SERVER_PASSWORD = credentials("SECOND_SERVER_PASSWORD")
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
            def existingContainer = sh (
                script: "sshpass -p ${SECOND_SERVER_PASSWORD} ssh ${SECOND_SERVER_USERNAME}@${SECOND_SERVER_IP} 'docker ps -q --filter name=java-app-container'",
                returnStdout: true
            ).trim()

            if (existingContainer) {
                sh "sshpass -p ${SECOND_SERVER_PASSWORD} ssh ${SECOND_SERVER_USERNAME}@${SECOND_SERVER_IP} 'docker rm -f ${existingContainer}'"
                echo "java-app-container başarıyla silindi."
            } else {
                echo "java-app-container bulunamadığı için silme işlemi yapılmayacak."
            }
            
            sh "sshpass -p ${SECOND_SERVER_PASSWORD} ssh -o StrictHostKeyChecking=no ${SECOND_SERVER_USERNAME}@${SECOND_SERVER_IP} 'docker pull altunarali/jenkins1:tag123'"
            sh "sshpass -p ${SECOND_SERVER_PASSWORD} ssh -o StrictHostKeyChecking=no ${SECOND_SERVER_USERNAME}@${SECOND_SERVER_IP} 'docker run -d --name java-app-container -p 8080:8080 altunarali/jenkins1:tag123'"
        }
    }
}

    }
}

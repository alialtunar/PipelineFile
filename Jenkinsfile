pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = credentials("DOCKER_IMAGE_NAME")
        SECOND_SERVER_USERNAME = credentials("SECOND_SERVER_USERNAME")
        DOCKERHUB_USERNAME =  credentials("DOCKERHUB_USERNAME")
        SECOND_SERVER_IP = credentials("SECOND_SERVER_IP")
        SECOND_SERVER_PASSWORD = credentials("SECOND_SERVER_PASSWORD")
        PATH = "$PATH:/opt/apache-maven-3.9.6/bin"
    }
    
    stages {
        
       stage('Deploy to Second Linux Server') {
    steps {
        script {
            def existingContainer = sh (
                script: "sshpass -p '" + SECOND_SERVER_PASSWORD + "' ssh '" + SECOND_SERVER_USERNAME + "@" + SECOND_SERVER_IP + "' 'docker ps -q --filter name=java-app-container'",
                returnStdout: true
            ).trim()

            if (existingContainer) {
                sh "sshpass -p '" + SECOND_SERVER_PASSWORD + "' ssh '" + SECOND_SERVER_USERNAME + "@" + SECOND_SERVER_IP + "' 'docker stop ${existingContainer}'"
                echo "java-app-container durduruldu."
                
                sh "sshpass -p '" + SECOND_SERVER_PASSWORD + "' ssh '" + SECOND_SERVER_USERNAME + "@" + SECOND_SERVER_IP + "' 'docker rm ${existingContainer}'"
                echo "java-app-container başarıyla silindi."
            } else {
                echo "java-app-container bulunamadığı için silme işlemi yapılmayacak."
            }

            // Konteyner durduruldu ve silindi mi kontrol edilir
            def stoppedContainer = sh (
                script: "sshpass -p '" + SECOND_SERVER_PASSWORD + "' ssh '" + SECOND_SERVER_USERNAME + "@" + SECOND_SERVER_IP + "' 'docker ps -q -a --filter name=java-app-container'",
                returnStdout: true
            ).trim()

            if (stoppedContainer) {
                // Mevcut konteyner durduruldu ve silindi, şimdi yeni bir konteyner oluşturabiliriz
                sh "sshpass -p '" + SECOND_SERVER_PASSWORD + "' ssh '" + SECOND_SERVER_USERNAME + "@" + SECOND_SERVER_IP + "' 'docker pull altunarali/jenkins1:tag123'"
                sh "sshpass -p '" + SECOND_SERVER_PASSWORD + "' ssh '" + SECOND_SERVER_USERNAME + "@" + SECOND_SERVER_IP + "' 'docker run -d --name java-app-container -p 8080:8080 altunarali/jenkins1:tag123'"
            } else {
                // Mevcut konteyner zaten durmuş ve silinmiş, sadece yeni bir konteyner oluşturabiliriz
                sh "sshpass -p '" + SECOND_SERVER_PASSWORD + "' ssh '" + SECOND_SERVER_USERNAME + "@" + SECOND_SERVER_IP + "' 'docker pull altunarali/jenkins1:tag123'"
                sh "sshpass -p '" + SECOND_SERVER_PASSWORD + "' ssh '" + SECOND_SERVER_USERNAME + "@" + SECOND_SERVER_IP + "' 'docker run -d --name java-app-container -p 8080:8080 altunarali/jenkins1:tag123'"
            }
        }
    }
}



    }
}

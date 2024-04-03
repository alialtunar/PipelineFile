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
                script: "sshpass -p '" + SECOND_SERVER_PASSWORD + "' ssh '" + SECOND_SERVER_USERNAME + "@" + SECOND_SERVER_IP + "' 'docker ps -aq --filter name=java-app-container'",
                returnStdout: true
            ).trim()

            if (existingContainer) {
                def stopResult = sh "sshpass -p '" + SECOND_SERVER_PASSWORD + "' ssh '" + SECOND_SERVER_USERNAME + "@" + SECOND_SERVER_IP + "' 'docker stop ${existingContainer}'"
                if (stopResult.exitStatus != 0) {
                    error "java-app-container durdurulurken bir hata oluştu."
                }
                echo "java-app-container durduruldu."

                def removeResult = sh "sshpass -p '" + SECOND_SERVER_PASSWORD + "' ssh '" + SECOND_SERVER_USERNAME + "@" + SECOND_SERVER_IP + "' 'docker rm ${existingContainer}'"
                if (removeResult.exitStatus != 0) {
                    error "java-app-container silinirken bir hata oluştu."
                }
                echo "java-app-container başarıyla silindi."
            } else {
                echo "java-app-container bulunamadığı için silme işlemi yapılmayacak."
            }

            sh "sshpass -p '" + SECOND_SERVER_PASSWORD + "' ssh '" + SECOND_SERVER_USERNAME + "@" + SECOND_SERVER_IP + "' 'docker pull altunarali/jenkins1:tag123'"
            sh "sshpass -p '" + SECOND_SERVER_PASSWORD + "' ssh '" + SECOND_SERVER_USERNAME + "@" + SECOND_SERVER_IP + "' 'docker run -d --name java-app-container -p 8080:8080 altunarali/jenkins1:tag123'"
        }
    }
}


    }
}

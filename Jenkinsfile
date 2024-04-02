pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = "java-app-xxx102144"
        DOCKER_HUB_ACCESS_TOKEN = "dckr_pat_ZJi4yNE8yUqcnhQwFWBId5oua7s" 
        SECOND_SERVER_USERNAME = "altunarali"
        DOCKERHUB_USERNAME = "altunarali"
        SECOND_SERVER_IP = "10.0.2.7"
        SECOND_SERVER_PASSWORD = "debian"
       PATH = "$PATH:/usr/bin"



    }
 stages {
        stage('Deploy to Second Linux Server') {
            steps {
                script {
                    def existingContainer = sh (
                        script: "sshpass -p ${SECOND_SERVER_PASSWORD} ssh ${SECOND_SERVER_USERNAME}@${SECOND_SERVER_IP} 'docker ps -q --filter name=java-app-container'",
                        returnStdout: true
                    ).trim()

                    if (existingContainer) {
                        sh "sshpass -p ${SECOND_SERVER_PASSWORD} ssh  ${SECOND_SERVER_USERNAME}@${SECOND_SERVER_IP} 'docker rm -f ${existingContainer}'"
                    }
                    
                    sh "sshpass -p ${SECOND_SERVER_PASSWORD} ssh -o StrictHostKeyChecking=no ${SECOND_SERVER_USERNAME}@${SECOND_SERVER_IP} 'docker pull altunarali/jenkins1:tag123'"
                    sh "sshpass -p ${SECOND_SERVER_PASSWORD} ssh -o StrictHostKeyChecking=no ${SECOND_SERVER_USERNAME}@${SECOND_SERVER_IP} 'docker run -d --name java-app-container -p 8080:8080 altunarali/jenkins1:tag123'"
                }
            }
        }
    }
}

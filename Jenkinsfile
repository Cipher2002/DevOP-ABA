pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git clone 'https://github.com/Cipher2002/DevOP-ABA.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("devop-aba:${env.BUILD_ID}")
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    dockerImage.run("-p 8080:80")
                }
            }
        }
    }
    post {
        always {
            script {
                // Clean up the Docker container and image
                sh "docker ps -a -q --filter ancestor=devop-aba:${env.BUILD_ID} | xargs docker rm -f"
                sh "docker rmi devop-aba:${env.BUILD_ID}"
            }
        }
    }
}

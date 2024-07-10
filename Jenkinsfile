pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                it credentialsId: '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', url: 'https://github.com/Cipher2002/DevOP-ABA.git'
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
                    dockerImage.run("-p 8080:81")
                }
            }
        }
    }
}

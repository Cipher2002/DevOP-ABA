pipeline {
    agent any
    environment {
        dockerImage = ''
        registry = 'devop-aba'
        registryCredential = 'anshchauhan2002'
        dockerRunPort = '8089:80' // Map the host port 8081 to the container port 80
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/master']],
                    extensions: [],
                    userRemoteConfigs: [[url: 'https://github.com/Cipher2002/DevOP-ABA.git']]
                )
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build(registry)
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {                    
                    // Run the new container on port 8081
                    def containerId = dockerImage.run("-p ${dockerRunPort}").id
                    echo "Container is running at: http://localhost:8089"
                }
            }
        }
    }
}

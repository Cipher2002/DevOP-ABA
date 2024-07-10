pipeline {
    agent any
    environment {
        dockerImage = ''
        registry = 'devop-aba'
        registryCredential = 'dckr_pat_QigUP8hIzmm9lVRwMrEEMG6TRik'
        dockerRunPort = '8081:80' // Map the host port 8081 to the container port 80
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
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push("${env.BUILD_ID}")
                        dockerImage.push("latest")
                    }
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {                    
                    // Run the new container on port 8081
                    def containerId = dockerImage.run("-p ${dockerRunPort}").id
                    echo "Container is running at: http://localhost:8081"
                }
            }
        }
    }
    post {
        always {
            script {
                // Clean up Docker images after build
                sh "docker rmi ${dockerImage.id} || true"
            }
        }
    }
}

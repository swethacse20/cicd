pipeline {
    agent any

    environment {
        // Define Docker Hub credentials ID
        DOCKERHUB_CREDENTIALS = 'docker-hub-credentials'
        // Define Docker image details
        DOCKER_IMAGE = 'swetha328/myhtmlapp'
        // Define Docker container name
        CONTAINER_NAME = 'mycontainer'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Git repository
                git 'https://github.com/swethacse20/registration.git'
            }
        }

        stage('Build') {
            steps {
                // Build Docker image
                script {
                    docker.build("${DOCKER_IMAGE}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push') {
            steps {
                // Push Docker image to Docker Hub
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'DOCKERHUB_CREDENTIALS') {
                        docker.image("${DOCKER_IMAGE}:${BUILD_NUMBER}").push()
                    }
                }
            }
        }

        stage('Pull') {
            steps {
                // Pull Docker image from Docker Hub
                script {
                    docker.pull("${DOCKER_IMAGE}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Run') {
            steps {
                // Run Docker container
                script {
                    docker.run("${DOCKER_IMAGE}:${BUILD_NUMBER}", '--name ${CONTAINER_NAME} -d -p 8080:80')
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline successfully completed!'
        }
        failure {
            echo 'Pipeline failed :('
        }
        always {
            // Clean up: remove Docker containers and images
            script {
                docker.stop(CONTAINER_NAME)
                docker.remove(CONTAINER_NAME)
                docker.image("${DOCKER_IMAGE}:${BUILD_NUMBER}").remove()
            }
        }
    }
}

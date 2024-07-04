pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "devopsproject:latest"
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Build Docker image
                    sh 'docker-compose build'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Test inside Docker container
                    sh 'docker-compose run app npm install'
                    sh 'docker-compose run app npm test'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Stop and remove existing containers (if any)
                    sh 'docker-compose down'

                    // Run Docker containers from the latest image
                    sh 'docker-compose up -d'
                }
            }
        }
    }
}
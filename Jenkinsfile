pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "devopsproject:latest"
    }

    stages {
        stage('Build') {
            steps {
                script {
                    dockerImage = docker.build(env.DOCKER_IMAGE)
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'npm install'
                        sh 'npm test'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'npm start &'
                    }
                }
            }
        }
    }
}

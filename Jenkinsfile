pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'git@github.com:ajmalnaim8/devopsproject.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("devopsproject:latest")
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    dockerImage.inside {
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

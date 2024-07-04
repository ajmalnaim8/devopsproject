pipeline {
    agent any

    parameters {
        choice(name: 'ENVIRONMENT', choices: ['linux', 'windows'], description: 'Choose the environment')
    }

    environment {
        DOCKER_IMAGE = "devopsproject:latest"
    }

    stages {
        stage('Build') {
            steps {
                script {
                    if (params.ENVIRONMENT == 'linux') {
                        sh 'docker-compose build'
                    } else if (params.ENVIRONMENT == 'windows') {
                        bat 'docker-compose build'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    if (params.ENVIRONMENT == 'linux') {
                        sh 'docker-compose run app npm install'
                        sh 'docker-compose run app npm test'
                    } else if (params.ENVIRONMENT == 'windows') {
                        bat 'docker-compose run app npm install'
                        bat 'docker-compose run app npm test'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (params.ENVIRONMENT == 'linux') {
                        sh 'docker-compose down'
                        sh 'docker-compose up -d'
                    } else if (params.ENVIRONMENT == 'windows') {
                        bat 'docker-compose down'
                        bat 'docker-compose up -d'
                    }
                }
            }
        }
    }
}
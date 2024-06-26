pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "devopsproject:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
                          doGenerateSubmoduleConfigurations: false,
                          extensions: [],
                          submoduleCfg: [],
                          userRemoteConfigs: [[credentialsId: 'your-credentials-id',
                                               url: 'https://github.com/ajmalnaim8/devopsproject.git']]])
            }
        }

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
                        // Use the home directory of the Jenkins user in the container
                        sh 'mkdir -p ~/.npm-global'
                        sh 'mkdir -p ~/.npm-cache'
                        // Set environment variables for npm configuration
                        withEnv(['NPM_CONFIG_PREFIX=~/.npm-global', 'NPM_CONFIG_CACHE=~/.npm-cache']) {
                            sh 'npm install'
                            sh 'npm test'
                        }
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

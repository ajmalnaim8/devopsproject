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
                          userRemoteConfigs: [[credentialsId: 'githubid',
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
                        // Use explicit path for Jenkins home directory inside the container
                        def npmGlobalDir = "/var/jenkins_home/.npm-global"
                        def npmCacheDir = "/var/jenkins_home/.npm-cache"
                        sh "mkdir -p ${npmGlobalDir}"
                        sh "mkdir -p ${npmCacheDir}"
                        // Set environment variables for npm configuration
                        withEnv(["NPM_CONFIG_PREFIX=${npmGlobalDir}", "NPM_CONFIG_CACHE=${npmCacheDir}"]) {
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

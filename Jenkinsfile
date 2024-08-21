pipeline {
    agent any

    parameters {
        string(name: 'DOCKER_TAG', defaultValue: 'latest', description: 'Tag for the Docker image')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    docker.image('node:16-alpine').inside {
                        dir('app') {  // Directory where package.json is located
                            sh 'npm install'
                        }
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.image('node:16-alpine').inside {
                        dir('app') {  // Directory where index.js is located
                            sh 'node index.js'
                        }
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("my-node-app:${params.DOCKER_TAG}", 'app')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-credentials-id') {
                        docker.image("my-node-app:${params.DOCKER_TAG}").push()
                    }
                }
            }
        }
    }
}

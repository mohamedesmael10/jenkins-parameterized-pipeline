pipeline {
    agent any

    environment {
        // Ensure this ID matches the ID of your Docker Hub credentials in Jenkins
        DOCKER_CREDENTIALS_ID = 'dockerhub'  
        // Default Docker Hub registry URL
        DOCKER_REGISTRY = 'index.docker.io'  
    }

    stages {
        stage('Clone') {
            steps {
                script {
                    // Clone the repository into the workspace
                    git url: 'https://github.com/mohamedesmael10/jenkins-parameterized-pipeline.git', branch: 'main'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Use a Docker container to install npm dependencies
                    docker.image('node:16-alpine').inside {
                        dir('app') {  // Ensure 'app' directory is correct
                            sh 'npm install'
                        }
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Use a Docker container to run the application
                    docker.image('node:16-alpine').inside {
                        dir('app') {  // Ensure 'app' directory is correct
                            sh 'node index.js'
                        }
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image from the 'app' directory
                    docker.build('mohamedesmael/jenkins-parameterized-pipeline:latest', 'app/')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image('mohamedesmael/jenkins-parameterized-pipeline:latest').push('latest')
                    }
                }
            }
        }
    }
}

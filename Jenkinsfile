pipeline {
    agent any

    parameters {
        booleanParam(name: 'DOCKER_PUSH', defaultValue: false, description: 'Set to true to enable Docker image push.')
    }

    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub' // Your Docker Hub credentials ID
        DOCKER_REGISTRY = 'docker.io'
    }

    stages {
        stage('Clone') {
            steps {
                script {
                    git url: 'https://github.com/mohamedesmael10/jenkins-parameterized-pipeline.git', branch: 'main'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    docker.image('node:16-alpine').inside {
                        dir('app') {  // Change to the directory where package.json is located
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
                        dir('app') {  // Change to the directory where index.js is located
                            sh 'node index.js'
                        }
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('mohamedesmael/jenkins-parameterized-pipeline:latest', 'app/')
                }
            }
        }

        stage('Push Docker Image') {
            when {
                expression {
                    return params.DOCKER_PUSH
                }
            }
            steps {
                script {
                    echo "Attempting to push Docker image..."
                    docker.withRegistry("https://${env.DOCKER_REGISTRY}", "${env.DOCKER_CREDENTIALS_ID}") {
                        docker.image('mohamedesmael/jenkins-parameterized-pipeline:latest').push('latest')
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
        success {
            echo 'Pipeline succeeded.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

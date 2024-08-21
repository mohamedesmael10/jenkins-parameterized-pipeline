pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                script {
                    // Clone the repository into the specified directory
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
    }
}

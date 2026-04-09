pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        IMAGE_NAME = ''
        PORT = ''
        CONTAINER_NAME = ''
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Set Environment') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        env.IMAGE_NAME = 'nodemain:v1.0'
                        env.PORT = '3000'
                        env.CONTAINER_NAME = 'app_main'
                    } else if (env.BRANCH_NAME == 'dev') {
                        env.IMAGE_NAME = 'nodedev:v1.0'
                        env.PORT = '3001'
                        env.CONTAINER_NAME = 'app_dev'
                    } else {
                        error("Unsupported branch: ${env.BRANCH_NAME}")
                    }
                }
            }
        }

        stage('Check Node and NPM') {
            steps {
                bat 'echo PATH=%PATH%'
                bat 'node -v'
                bat 'npm -v'
                bat 'where node'
                bat 'where npm'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Test') {
            steps {
                bat 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Deploy') {
            steps {
                bat 'docker rm -f %CONTAINER_NAME%'
                bat 'docker run -d --name %CONTAINER_NAME% -p %PORT%:3000 %IMAGE_NAME%'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished'
        }
    }
}

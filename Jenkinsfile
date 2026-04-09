pipeline {
    agent any

    environment {
        NODE_EXE = 'C:\\Program Files\\nodejs\\node.exe'
        NPM_CMD = 'C:\\Program Files\\nodejs\\npm.cmd'
    }

    stages {
        stage('Set Environment') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        env.IMAGE_NAME = 'nodemain:v1.0'
                        env.PORT = '3000'
                        env.APP_PORT = '3000'
                        env.CONTAINER_NAME = 'app_main'
                    } else if (env.BRANCH_NAME == 'dev') {
                        env.IMAGE_NAME = 'nodedev:v1.0'
                        env.PORT = '3001'
                        env.APP_PORT = '3001'
                        env.CONTAINER_NAME = 'app_dev'
                    } else {
                        error("Unsupported branch: ${env.BRANCH_NAME}")
                    }

                    echo "BRANCH_NAME = ${env.BRANCH_NAME}"
                    echo "IMAGE_NAME = ${env.IMAGE_NAME}"
                    echo "PORT = ${env.PORT}"
                    echo "CONTAINER_NAME = ${env.CONTAINER_NAME}"
                }
            }
        }

        stage('Check Node and NPM') {
            steps {
                bat '"%NODE_EXE%" -v'
                bat '"%NPM_CMD%" -v'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '"%NPM_CMD%" install'
            }
        }

        stage('Test') {
            steps {
                bat '"%NPM_CMD%" test'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    bat "docker build -t ${env.IMAGE_NAME} ."
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    bat "docker rm -f ${env.CONTAINER_NAME} || ver > nul"
                    bat "docker run -d --name ${env.CONTAINER_NAME} -p ${env.PORT}:3000 ${env.IMAGE_NAME}"
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished'
        }
    }
}

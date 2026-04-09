pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: "${env.BRANCH_NAME}", url: 'https://github.com/AryenSGarg/jenkins-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Test') {
            steps {
                bat 'echo Running Tests...'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        bat 'docker build -t myapp:main .'
                    } else {
                        bat 'docker build -t myapp:dev .'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        bat 'echo Deploying MAIN on port 3000'
                    } else {
                        bat 'echo Deploying DEV on port 3001'
                    }
                }
            }
        }

    }
}

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

        stage('Run App') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        bat 'echo Running MAIN branch on port 3000'
                    } else {
                        bat 'echo Running DEV branch on port 3001'
                    }
                }
            }
        }

    }
}

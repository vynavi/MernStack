pipeline {
    agent any

    environment {
        PATH = "C:\\Program Files\\nodejs;${env.PATH}" // Update this if Node.js is installed elsewhere
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    deleteDir()  // Delete workspace before checkout
                    checkout scm
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    bat 'npm install'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    bat """
                    sonar-scanner ^ 
                        -Dsonar.projectKey=task1 ^ 
                        -Dsonar.sources=. ^ 
                        -Dsonar.host.url=http://localhost:9000 ^ 
                        -Dsonar.token=${SONAR_TOKEN}
                    """
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    bat 'npm run build'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    bat 'npm test'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}

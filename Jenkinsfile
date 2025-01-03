pipeline {
    agent any
    
    environment {
        // Correct path to Node.js installation on Windows
        NODE_PATH = 'C:/Program Files/nodejs'
        // Add Node.js and npm to the system PATH variable
        PATH = "${NODE_PATH};C:/Program Files/nodejs/node_modules/npm;%PATH%"
        SONAR_TOKEN = credentials('sonar_token') // SonarQube token stored in Jenkins credentials
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
                    // Use bat for Windows to run npm commands
                    bat 'npm install'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Use bat to run SonarQube scanner on Windows
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
                    // Use bat to run build command
                    bat 'npm run build'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Use bat to run tests with npm
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

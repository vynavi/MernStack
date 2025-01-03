pipeline {
    agent any
    tools {
        nodejs 'NodeJS' // Node.js tool name configured in Jenkins
    }
    environment {
        SONAR_SCANNER_HOME = 'C:\\Program Files\\sonarqube-10.7.0.96327\\bin\\windows-x86-64'
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_PROJECT_KEY = 'task1'
        SONAR_PROJECT_NAME = 'task1'
        SONAR_TOKEN = credentials('sonar_token') // SonarQube token stored in Jenkins credentials
        PATH = "C:\\Windows\\System32;C:\\Program Files\\Git\\bin"
        NODE_PATH = 'C:/Program Files/nodejs;C:/Program Files/nodejs/node_modules/npm'  // Path to Node.js installation folder
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/vynavi/MernStack.git'
                script {
                    deleteDir()  // Clean workspace before checkout
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install' // Run npm install only once
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube analysis using the sonar-scanner
                    bat """
                    C:\\Program Files\\sonarqube-10.7.0.96327\\bin\\windows-x86-64\\sonar-scanner.bat ^
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
                bat 'npm run build' // Run build command
            }
        }
        stage('Test') {
            steps {
                bat 'npm test' // Run tests
            }
        }
    }
    post {
        always {
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'SonarQube analysis completed successfully!'
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline execution failed!'
            echo 'Pipeline failed'
        }
    }
}

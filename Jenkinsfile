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
                script {
                    // Ensure we're on the correct branch and repository
                    echo 'Checking out the repository...'
                    git branch: 'main', url: 'https://github.com/vynavi/MernStack/tree/main/backend.git'
                    deleteDir()  // Clean workspace before checking out code
                }
            }
        }
        stage('Verify Directory and File Existence') {
            steps {
                script {
                    // Check if the 'backend' directory exists in the correct path
                    //echo 'Checking if backend directory exists...'
                    //if (fileExists('backend')) {
                    //    echo "'backend' directory found."
                    //} else {
                    //    error "'backend' directory does not exist in the repository."
                    //}
                    
                    // Check if 'package.json' exists inside the 'backend' directory
                    echo 'Checking if package.json exists...'
                    if (fileExists('package.json')) {
                        echo "'package.json' found in the 'backend' directory."
                    } else {
                        error "'package.json' file is missing in the 'backend' directory."
                    }
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    // Navigate to the backend directory and install dependencies
                    echo 'Installing dependencies...'
                    dir('backend') {
                        bat 'npm install' // Run npm install inside 'backend' directory
                    }
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube analysis using the sonar-scanner from the backend directory
                    echo 'Running SonarQube analysis...'
                    dir('backend') {
                        bat """
                        ${SONAR_SCANNER_HOME}\\sonar-scanner.bat ^
                        -Dsonar.projectKey=${SONAR_PROJECT_KEY} ^
                        -Dsonar.sources=. ^
                        -Dsonar.host.url=${SONAR_HOST_URL} ^
                        -Dsonar.token=${SONAR_TOKEN}
                        """
                    }
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    // Run build command only if 'npm install' was successful, inside the 'backend' directory
                    echo 'Running build...'
                    dir('backend') {
                        bat 'npm run build' // Run build command inside 'backend'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    // Run tests inside the 'backend' directory
                    echo 'Running tests...'
                    dir('backend') {
                        bat 'npm test' // Run tests inside 'backend'
                    }
                }
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

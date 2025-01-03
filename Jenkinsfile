pipeline {
    agent any
    tools {
        nodejs 'NodeJS' 
    }
    environment {
        SONAR_SCANNER_HOME = 'C:\\Program Files\\sonarqube-10.7.0.96327\\bin\\windows-x86-64'
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_PROJECT_KEY = 'task1'
        SONAR_PROJECT_NAME = 'task1'
        SONAR_TOKEN = credentials('sonar_token') 
        PATH = "C:\\Windows\\System32;C:\\Program Files\\Git\\bin"
        NODE_PATH = 'C:/Program Files/nodejs;C:/Program Files/nodejs/node_modules/npm'  
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    
                    echo 'Checking out the repository...'
                    git branch: 'main', url: 'https://github.com/vynavi/MernStack/tree/main/backend.git'
                    deleteDir()  
                }
            }
        }
        stage('Verify Directory and File Existence') {
            steps {
                script {
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
                    echo 'Installing dependencies...'
                    dir('backend') {
                        bat 'npm install' 
                    }
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
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
                    echo 'Running build...'
                    dir('backend') {
                        bat 'npm run build' 
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    dir('backend') {
                        bat 'npm test' 
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

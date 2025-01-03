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
        SONAR_TOKEN = 'sqp_7489e020b5ce143c76ccaabdfff00b6ff25ea184'
        PATH = "C:\\Windows\\System32;C:\\Program Files\\Git\bin"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/vynavi/MernStack.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar_token')
            }
            steps {
                bat """
                sonar-scanner ^
                -Dsonar.projectKey=task1 ^
                -Dsonar.sources=. ^
                -Dsonar.host.url=http://localhost:9000 ^
                -Dsonar.token=%SONAR_TOKEN%
                """
            }
        }
    }
    post {
        always {
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'SonarQube analysis completed successfully!'
        }
        failure {
            echo 'Pipeline execution failed!'
        }
    }
}

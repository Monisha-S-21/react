pipeline {
    agent any
    tools {
        nodejs 'nodejs-18.16.1' // Name of the Node.js installation in Jenkins
    }

    environment {
        NODEJS_HOME = 'C:/Program Files/nodejs'  // Set the Node.js path
        SONAR_SCANNER_PATH = 'C:/Users/Monisha/Downloads/sonar-scanner-6.2.1.4610-windows-x64/bin'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Set the PATH and install dependencies using npm
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm install
                '''
            }
        }

        stage('Lint') {
            steps {
                // Run linting to ensure code quality
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm run lint
                '''
            }
        }

        stage('Build') {
            steps {
                // Build the React app
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm run build
                '''
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar') // Accessing the SonarQube token stored in Jenkins credentials
            }
            steps {
                // Ensure that sonar-scanner is in the PATH
                bat '''
                set PATH=%SONAR_SCANNER_PATH%;%PATH%
                where sonar-scanner || echo "SonarQube scanner not found. Please install it."
                sonar-scanner.bat -D"sonar.projectKey=register" -D"sonar.sources=." -D"sonar.host.url=http://localhost:9000" -D"sonar.token=sqp_96f8ea2e88bd9c84a07bc53d6fab04aa3d62e7da"
                '''
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
        always {
            echo 'This runs regardless of the result.'
        }
    }
}

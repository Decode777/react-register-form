pipeline {
    agent any
    tools {
        nodejs 'nodejs'
    }
    environment {
        NODEJS_HOME = 'C:/Program Files/nodejs'
        SONAR_SCANNER_PATH = 'C:\\Users\\vaish\\Downloads\\sonar-scanner-cli-6.2.1.4610-windows-x64\\sonar-scanner-6.2.1.4610-windows-x64\\bin'
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
                SONAR_TOKEN = credentials('sonar-token') // Accessing the SonarQube token stored in Jenkins 
            }
            steps {
                // Ensure that sonar-scanner is in the PATH
                bat '''
                set PATH=%SONAR_SCANNER_PATH%;%PATH%
                where sonar-scanner || echo "SonarQube scanner not found. Please install it."
                sonar-scanner.bat 
                -D"sonar.projectKey=react-form" 
                -D"sonar.sources=." 
                -D"sonar.host.url=http://localhost:9000" 
                -D"sonar.token=sqp_fa35ac138a204adcbe4f395b52f518cbd0a1e3f6"
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

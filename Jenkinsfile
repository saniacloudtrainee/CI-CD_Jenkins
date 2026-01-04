pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python') {
            steps {
                bat '''
                set PYTHON_HOME=C:\\Users\\lenovo\\AppData\\Local\\Programs\\Python\\Python311
                set PATH=%PYTHON_HOME%;%PYTHON_HOME%\\Scripts;%PATH%
                set PYTHONPATH=%CD%
                python --version
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                python -m venv venv
                call venv\\Scripts\\activate
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                bat '''
                call venv\\Scripts\\activate
                pytest
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Build successful – tests passed'
        }
        failure {
            echo '❌ Build failed – tests failed'
        }
    }
}

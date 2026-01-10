pipeline {
    agent any

    environment {
        PYTHON_HOME = 'C:\\Users\\lenovo\\AppData\\Local\\Programs\\Python\\Python311'
        PATH = "${PYTHON_HOME};${PYTHON_HOME}\\Scripts;${env.PATH}"
        PYTHONPATH = "${WORKSPACE}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Verify Python') {
            steps {
                bat 'python --version'
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
        
        stage('Docker Version Check') {
            steps {
                bat 'docker --version'
            }
        }
   
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t jenkins-ci-demo .'
            }
        }
    }

    post {
        success {
            mail to: 'sania.cloudtech@gmail.com',
                 subject: "Jenkins SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: """\
✅ Build SUCCESS

Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}

View details:
${env.BUILD_URL}
"""
        }

        failure {
            mail to: 'sania.cloudtech@gmail.com',
                 subject: "Jenkins FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: """\
❌ Build FAILED

Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}

Check logs:
${env.BUILD_URL}
"""
        }
    }
}


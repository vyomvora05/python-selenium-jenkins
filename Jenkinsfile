pipeline {
    agent any

    stages {
        stage('Checkout Source Code') {
            steps {
                checkout scm
            }
        }

        stage('Set Up Environment & Dependencies') {
            steps {
                sh '''
                    echo "Creating virtual environment..."
                    rm -rf venv
                    python3 -m venv venv

                    echo "Installing dependencies..."
                    venv/bin/python -m pip install --upgrade pip
                    venv/bin/python -m pip install -r requirements.txt
                '''
            }
        }

        stage('Execute Selenium Tests') {
            steps {
                sh '''
                    mkdir -p reports
                    echo "Running Pytest Suite..."
                    venv/bin/python -m pytest tests/ --junitxml=reports/junit-report.xml
                '''
            }
        }
    }

    post {
        always {
            junit testResults: 'reports/junit-report.xml',
                  allowEmptyResults: true
        }
    }
}

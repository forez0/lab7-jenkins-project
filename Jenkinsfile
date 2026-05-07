pipeline {
    agent any

    environment {
        // Шлях до папки доставки на вашому комп'ютері
        DELIVERY_DIR = "${WORKSPACE}/cargo/project_build"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Завантаження коду з GitHub...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Встановлення залежностей...'
                bat '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Запуск тестів...'
                bat '''
                    . venv/bin/activate
                    pytest tests/ --junitxml=test-results/results.xml
                '''
            }
            post {
                always {
                    junit 'test-results/results.xml'
                }
            }
        }

        stage('Deliver') {
            steps {
                echo 'Доставка у папку cargo...'
                bat '''
                    mkdir -p ${DELIVERY_DIR}
                    cp -r src/ ${DELIVERY_DIR}/
                    cp requirements.txt ${DELIVERY_DIR}/
                    echo "Доставлено до: ${DELIVERY_DIR}"
                '''
            }
        }
    }
}
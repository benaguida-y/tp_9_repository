pipeline {
    agent any

    options {
        skipDefaultCheckout(false)
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout') {
            steps {
		git branch:'main',
                url: 'https://github.com/benaguida-y/tp_9_repository.git',
		credentialsId: 'github-token'
            }
        }

        stage('Build + Tests + Sécurité') {
            steps {
                sh '''
                echo "🔧 Creating virtual environment..."
                rm -rf venv
                python3 -m venv venv

                echo "⬆️ Upgrading pip..."
                venv/bin/python -m pip install --upgrade pip --break-system-packages

                echo "📦 Installing dependencies..."
                venv/bin/pip install -r requirements.txt

                echo "🧪 Running tests..."
                venv/bin/pytest tests/ || exit 1

                echo "🔐 Installing security tools..."
                venv/bin/pip install bandit safety

                echo "🔍 Running Bandit (code security scan)..."
                venv/bin/bandit -r . || true

                echo "📋 Running Safety (dependency vulnerabilities)..."
                venv/bin/safety check || true
                '''
            }
        }

        stage('Cleanup') {
            steps {
                echo "🧹 Cleaning workspace..."
                cleanWs()
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline réussi"
        }
        failure {
            echo "❌ Pipeline échoué"
        }
    }
}

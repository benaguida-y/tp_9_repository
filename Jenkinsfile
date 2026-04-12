pipeline {
    agent any

    stages {

        stage('Build + Tests + Sécurité') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                    pytest tests/

                    pip install bandit safety
                    bandit -r src/ -ll
                    safety check
                '''
            }
        }

        stage('Nettoyage') {
            steps {
                sh 'rm -rf venv'
            }
        }
    }

    post {
        success {
            echo "Pipeline exécuté avec succès."
        }
        failure {
            echo "Pipeline échoué"
        }
    }
}

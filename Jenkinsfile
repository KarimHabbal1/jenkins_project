pipeline {
    agent any
    environment { VIRTUAL_ENV = 'venv' }

    stages {
        stage('Setup') {
            steps {
                script {
                    if (!fileExists("${env.WORKSPACE}/${VIRTUAL_ENV}")) {
                        sh "python3 -m venv ${VIRTUAL_ENV}"
                    }
                    sh "source ${VIRTUAL_ENV}/bin/activate && pip install -r requirements.txt"
                }
            }
        }

        stage('Lint') {
            steps {
                sh "source ${VIRTUAL_ENV}/bin/activate && flake8 app.py"
            }
        }

        stage('Test') {
            steps {
                sh "source ${VIRTUAL_ENV}/bin/activate && pytest"
            }
        }

        stage('Coverage') {
            steps {
                sh "source ${VIRTUAL_ENV}/bin/activate && coverage run -m pytest && coverage report"
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    sh "source ${VIRTUAL_ENV}/bin/activate && bandit -r . --exclude venv || true"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application..."
                    sh "cp app.py /tmp/deployed_app.py"
                    echo "Application deployed successfully."
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
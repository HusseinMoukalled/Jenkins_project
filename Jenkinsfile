pipeline {
    agent any

    environment {
        VIRTUAL_ENV = 'venv'
        PYTHONPATH = "${WORKSPACE}"
    }

    stages {
        stage('Setup') {
            steps {
                script {
                    // Create virtual environment if not already present
                    if (!fileExists("${env.WORKSPACE}\\${VIRTUAL_ENV}")) {
                        bat "python -m venv ${VIRTUAL_ENV}"
                    }
                    // Upgrade pip before installing dependencies
                    bat "call ${VIRTUAL_ENV}\\Scripts\\activate && python -m pip install --upgrade pip"
                    // Install all required dependencies
                    bat "call ${VIRTUAL_ENV}\\Scripts\\activate && pip install -r requirements.txt --upgrade-strategy eager"
                }
            }
        }

        stage('Lint') {
            steps {
                script {
                    bat "call ${VIRTUAL_ENV}\\Scripts\\activate && flake8 app.py"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    bat "set PYTHONPATH=%WORKSPACE% && call ${VIRTUAL_ENV}\\Scripts\\activate && pytest -q"
                }
            }
        }

        stage('Coverage') {
            steps {
                script {
                    bat "set PYTHONPATH=%WORKSPACE% && call ${VIRTUAL_ENV}\\Scripts\\activate && coverage run -m pytest"
                    bat "call ${VIRTUAL_ENV}\\Scripts\\activate && coverage report"
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    bat "call ${VIRTUAL_ENV}\\Scripts\\activate && bandit -r ."
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deployment step (simulated for this lab)...'
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

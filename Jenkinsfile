pipeline {
    agent any

    environment {
        VIRTUAL_ENV = 'venv'
        // make project root importable as a module
        PYTHONPATH = "${WORKSPACE}"
    }

    stages {
        stage('Setup') {
            steps {
                script {
                    if (!fileExists("${env.WORKSPACE}\\${VIRTUAL_ENV}")) {
                        bat "python -m venv ${VIRTUAL_ENV}"
                    }
                    bat "call ${VIRTUAL_ENV}\\Scripts\\activate && pip install -r requirements.txt"
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
                    // ensure PYTHONPATH is set for this process on Windows
                    bat "set PYTHONPATH=%WORKSPACE% && call ${VIRTUAL_ENV}\\Scripts\\activate && pytest -q"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying application...'
                }
            }
        }
    }

    post {
        always { cleanWs() }
    }
}

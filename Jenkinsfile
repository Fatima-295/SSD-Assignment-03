pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo '📦 Installing dependencies...'

                // Create virtual environment
                bat 'python -m venv venv'

                // Activate venv + install requirements
                bat '''
                    call venv\\Scripts\\activate
                    python -m pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Running tests...'

                bat '''
                    call venv\\Scripts\\activate
                    pytest --maxfail=1 --disable-warnings --quiet
                    if %ERRORLEVEL% neq 0 exit /b %ERRORLEVEL%
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying Flask App...'

                bat '''
                    call venv\\Scripts\\activate
                    start /B python app.py
                '''
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline Completed Successfully!'
        }
        failure {
            echo '❌ Pipeline Failed!'
        }
    }
}

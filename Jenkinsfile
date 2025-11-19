pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo '📦 Installing dependencies...'
                
                // Create virtual environment
                sh 'python -m venv venv'
                
                // Activate venv + install requirements
                sh '''
                    source venv/Scripts/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Running tests...'
                
                // If you have pytest or unittests
                sh '''
                    source venv/Scripts/activate
                    if pytest --maxfail=1 --disable-warnings --quiet; then
                        echo "Tests passed"
                    else
                        echo "Tests failed"
                        exit 1
                    fi
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying Flask App...'
                
                // Run Flask app (or copy files to server)
                sh '''
                    source venv/Scripts/activate
                    python app.py &
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

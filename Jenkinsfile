pipeline {
    agent any

    parameters {
        string(name: 'VERSION', defaultValue: '', description: 'version to deploy on prod')
        choice(name: 'VERSION_CHOICE', choices: ['1.1.0', '1.2.0', '1.3.0'], description: 'Choose version')
        booleanParam(name: 'executeTests', defaultValue: true, description: 'Run Test Stage?')
    }

    environment {
        NEW_VERSION = '1.3.0'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out code from Git repository'
                checkout scm   // Make sure your Jenkins job is connected to your repo
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube scan...'
                withSonarQubeEnv('SonarServer') { // Replace with your configured SonarQube server name
                    bat '"D:\\ssd\\sonar-scanner-cli-7.3.0.5189-windows-x64\\sonar-scanner-7.3.0.5189-windows-x64\\bin\\sonar-scanner.bat" -Dsonar.projectKey=myproject -Dsonar.sources=. -Dsonar.host.url=%SONAR_HOST_URL% -Dsonar.login=%SONAR_AUTH_TOKEN%'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building Project'
                echo "Selected VERSION: ${params.VERSION}"
                echo "Selected VERSION_CHOICE: ${params.VERSION_CHOICE}"
                echo "Env NEW_VERSION: ${NEW_VERSION}"
                // Add your real build command here, e.g. bat 'mvn clean package'
            }
        }

        stage('Test') {
            when {
                expression {
                    params.executeTests
                }
            }
            steps {
                echo 'Testing Project'
                // Add your real test command here, e.g. bat 'mvn test'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

pipeline {
    agent any

    environment {
        VENV_DIR = "venv"
        DEPLOY_DIR = "C:/tmp/flask_app_deploy"  // Change path if you want
    }

    stages {

        stage('Clone Repository') {
            steps {
                echo 'Cloning GitHub repository...'
                // Git clone is handled automatically if using Pipeline from SCM
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing Python dependencies...'
                bat '''
                    python -m venv %VENV_DIR%
                    call %VENV_DIR%\\Scripts\\activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running unit tests...'
                bat '''
                    call %VENV_DIR%\\Scripts\\activate
                    pytest
                '''
            }
        }

        stage('Build Application') {
            steps {
                echo 'Building application...'
                bat '''
                    mkdir build
                    copy app.py build
                    copy requirements.txt build
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Deploying application (simulation)...'
                bat '''
                    mkdir %DEPLOY_DIR%
                    xcopy build\\* %DEPLOY_DIR% /E /Y
                    echo Application deployed to %DEPLOY_DIR%
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

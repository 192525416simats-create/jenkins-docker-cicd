pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-app"
        CONTAINER_NAME = "my-app-container"
        PORT = "8085"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out source code...'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image...'
                bat 'docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running Container Tests...'
                bat '''
                    if exist index.html (
                        echo Test Passed: index.html exists.
                    ) else (
                        echo Test Failed: index.html missing. && exit /b 1
                    )
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Deploying Container...'
                bat '''
                    docker stop %CONTAINER_NAME% || exit /b 0
                    docker rm %CONTAINER_NAME% || exit /b 0
                    docker run -d --name %CONTAINER_NAME% -p %PORT%:80 %IMAGE_NAME%:%BUILD_NUMBER%
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully! App is deployed.'
        }
        failure {
            echo 'Pipeline failed. Check build logs.'
        }
    }
}
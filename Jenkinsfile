pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'deepu09567'
        IMAGE_NAME = 'deepu09567/static-website5'
        IMAGE_TAG = 'latest'
        CONTAINER_NAME = 'static-website5'

        DOCKER = 'C:\\Users\\Ankit\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Pulling source code from GitHub...'

                git branch: 'main',
                    url: 'https://github.com/deepubhakuni5-create/staticwebsite5.git'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'

                bat """
                    "%DOCKER%" build -t %IMAGE_NAME%:%IMAGE_TAG% .
                """
            }
        }

        stage('Stop Old Container') {
            steps {
                echo 'Stopping old container if running...'

                bat """
                    "%DOCKER%" stop %CONTAINER_NAME% || exit /b 0
                    "%DOCKER%" rm %CONTAINER_NAME% || exit /b 0
                """
            }
        }

        stage('Run Container') {
            steps {
                echo 'Starting new container...'

                bat """
                    "%DOCKER%" run -d ^
                    --name %CONTAINER_NAME% ^
                    -p 8077:80 ^
                    %IMAGE_NAME%:%IMAGE_TAG%
                """
            }
        }
    }

    post {
        success {
            echo '======================================'
            echo 'Static Website Deployed Successfully!'
            echo 'Website: http://localhost:8077'
            echo '======================================'
        }

        failure {
            echo '======================================'
            echo 'Deployment Failed!'
            echo '======================================'
        }
    }
}


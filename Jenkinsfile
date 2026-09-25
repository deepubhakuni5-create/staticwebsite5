pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'deepu09567'
        IMAGE_NAME = 'deepu09567/static-website5'
        IMAGE_TAG = 'latest'
        CONTAINER_NAME = 'static-website5'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Pulling source code...'

                git branch: 'main',
                    url: 'https://github.com/deepubhakuni5-create/staticwebsite5.git'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'

                bat """
                    docker build -t %IMAGE_NAME%:%IMAGE_TAG% .
                """
            }
        }

        stage('Stop Old Container') {
            steps {
                echo 'Stopping old container if running...'

                bat """
                    docker stop %CONTAINER_NAME% || exit /b 0
                    docker rm %CONTAINER_NAME% || exit /b 0
                """
            }
        }

        stage('Run Container') {
            steps {
                echo 'Starting new container...'

                bat """
                    docker run -d ^
                    --name %CONTAINER_NAME% ^
                    -p 8077:80 ^
                    %IMAGE_NAME%:%IMAGE_TAG%
                """
            }
        }
    }

    post {
        success {
            echo 'Static website deployed successfully!'
            echo 'Open: http://localhost:8077'
        }

        failure {
            echo 'Deployment failed!'
        }
    }
}

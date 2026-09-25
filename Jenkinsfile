pipeline {
    agent any

    environment {
        IMAGE_NAME = 'deepu09567/static-website5'
        IMAGE_TAG = 'latest'
        CONTAINER_NAME = 'static-website5'

        DOCKER = 'C:\\Users\\Ankit\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe'

        PATH = "C:\\Users\\Ankit\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin;${env.PATH}"

        KUBECONFIG = 'C:\\Users\\Ankit\\.kube\\config'
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

        stage('Docker Login') {
            steps {
                echo 'Logging in to Docker Hub...'

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials-new',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    bat """
                        "%DOCKER%" login -u "%DOCKER_USER%" -p "%DOCKER_PASS%"
                    """
                }
            }
        }

        stage('Docker Push') {
            steps {
                echo 'Pushing image to Docker Hub...'

                bat """
                    "%DOCKER%" push %IMAGE_NAME%:%IMAGE_TAG%
                """
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                echo 'Deploying application to Kubernetes...'

                bat """
                    kubectl apply -f k8s.yaml
                """
            }
        }

        stage('Kubernetes Status') {
            steps {
                echo 'Checking Kubernetes resources...'

                bat """
                    kubectl get pods
                    kubectl get services
                """
            }
        }
    }

    post {
        success {
            echo '======================================'
            echo 'Deployment Successful!'
            echo '======================================'
        }

        failure {
            echo '======================================'
            echo 'Deployment Failed!'
            echo '======================================'
        }
    }
}


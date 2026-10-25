pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "deepaknr17/imagename"
        IMAGE_TAG = "latest" // Change this to "v1" if you prefer
        // Define the absolute path to Docker for Windows Jenkins service
        DOCKER_PATH = '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe"'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Deepak-NR17/ex.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "${DOCKER_PATH} build -t %DOCKER_IMAGE%:%IMAGE_TAG% ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(
                    credentialsId: 'dockerhub-PAT',
                    variable: 'DOCKER_PAT'
                )]) {
                    bat '''
                    @echo off
                    echo %DOCKER_PAT%| "C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" login -u deepaknr17 --password-stdin
                    if errorlevel 1 exit /b 1
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat "${DOCKER_PATH} push %DOCKER_IMAGE%:%IMAGE_TAG%"
            }
        }
    }

    post {
        success {
            echo 'Docker image built and pushed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}

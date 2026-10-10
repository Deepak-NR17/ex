
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "deepaknr17/myapp"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/Deepak-NR17/ex.git'
            }
        }

       stage('Build Docker Image') {
    steps {
        bat 'docker build -t %DOCKER_IMAGE%:latest .'
    }
}

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
                }
            }
        }

       stage('Push Docker Image') {
    steps {
        script {
            docker.withRegistry('', 'dockerhub-creds') {
                docker.image("${DOCKER_IMAGE}:latest").push()
            }
        }
    }
}
    }

    post {
        success {
            echo 'Image successfully built and pushed to Docker Hub'
        }

        failure {
            echo 'Pipeline failed'
        }
    }
}


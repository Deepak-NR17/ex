pipeline { agent any

environment { DOCKER_IMAGE = "deepaknr17/myapp" }

stages {

stage('Clone Repository') { steps { git 'https://github.com/Deepak-NR17/ex.git' } }

stage('Build Docker Image') { steps { script { docker.build("${DOCKER_IMAGE}:v1") } } }

stage('Login to Docker Hub') { steps { withCredentials([usernamePassword( credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKERUSER', passwordVariable: 'DOCKERPASS' )]) { bat 'echo %DOCKERPASS% | docker login -u %DOCKERUSER% --password-stdin' } } }

stage('Push Docker Image') { steps { script { docker.withRegistry('', 'dockerhub-creds') { docker.image("${DOCKER_IMAGE}:v1").push() } } } } }

post { success { echo 'Image successfully built and pushed to Docker Hub' }

failure { echo 'Pipeline failed' } } }

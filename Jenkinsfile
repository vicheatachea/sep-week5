pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    environment {
        // Define SonarQube server and token
        SONARQUBE_SERVER = 'SonarQubeServer'
        SONAR_TOKEN = 'squ_f9b17bf3e228f704610ccb22854427e7b0f3da0b'

        // Define Docker Hub credentials ID
        DOCKERHUB_CREDENTIALS_ID = 'DockerHub'
        // Define Docker Hub repository name
        DOCKERHUB_REPO = 'vicheatachea/sep2week5'
        // Define Docker image tag
        DOCKER_IMAGE_TAG = 'latest_v1'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/vicheatachea/sep-week5.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh """
                        sonar-scanner \
                        -Dsonar.projectKey=devops-demo \
                        -Dsonar.sources=src \
                        -Dsonar.projectName=DevOps-Demo \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=${env.SONAR_TOKEN} \
                        -Dsonar.java.binaries=target/classes
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS_ID) {
                        docker.image("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
}
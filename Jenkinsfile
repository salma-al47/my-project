pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerHubCredentials')  // ID des credentials Docker Hub stockés dans Jenkins
        DOCKER_IMAGE_BACKEND = "salmaal/my-project-backend"
        DOCKER_IMAGE_FRONTEND = "salmaal/my-project-frontend"
        DOCKER_IMAGE_POSTGRES = "postgres:latest"
    }

dockerHubCredentials

    stages {
        stage('Checkout') {
            steps {
                // Clone du repo Git
                git 'https://github.com/salma-al47/my-project.git'
            }
        }

        stage('Build Backend Image') {
            steps {
                script {
                    dir('./') {
                        sh 'docker build -t ${DOCKER_IMAGE_BACKEND} -f dockerfileboot .'
                    }
                }
            }
        }

        stage('Build Frontend Image') {
            steps {
                script {
                    dir('./src/main/webapp/reactjs/') {
                        sh 'docker build -t ${DOCKER_IMAGE_FRONTEND} -f dockerfilejs .'
                    }
                }
            }
        }

        stage('Push Backend Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-credentials') {
                        sh 'docker push ${DOCKER_IMAGE_BACKEND}'
                    }
                }
            }
        }

        stage('Push Frontend Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-credentials') {
                        sh 'docker push ${DOCKER_IMAGE_FRONTEND}'
                    }
                }
            }
        }
    }

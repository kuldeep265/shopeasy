pipeline {

    agent any

    environment {
        DOCKER_IMAGE = "kuldeep7860/shopeasy-frontend"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/kuldeep265/shopeasy.git'
            }
        }

        stage('Build Frontend Image') {
            steps {

                script {

                    dir('frontend') {

                        bat 'docker build -t %DOCKER_IMAGE%:latest .'

                    }
                }
            }
        }

        stage('Docker Hub Login') {

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

                bat 'docker push %DOCKER_IMAGE%:latest'

            }
        }

        stage('Deploy Containers') {

            steps {

                bat 'docker compose down'

                bat 'docker compose up -d'

            }
        }
    }
}
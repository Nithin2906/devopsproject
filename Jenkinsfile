pipeline {
    agent any

    environment {
        DOCKER_HUB = "nithinp004"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Nithin2906/devopsproject.git'
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                dir('backend') {
                    bat '''
                    docker build -t %DOCKER_HUB%/pd-backend:%BUILD_NUMBER% -t %DOCKER_HUB%/pd-backend:latest .
                    '''
                }
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                dir('frontend') {
                    bat '''
                    docker build -t %DOCKER_HUB%/pd-frontend:%BUILD_NUMBER% -t %DOCKER_HUB%/pd-frontend:latest .
                    '''
                }
            }
        }

        stage('Docker Hub Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    bat '''
                    docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                    '''
                }
            }
        }

        stage('Push Backend Image') {
            steps {
                bat '''
                docker push %DOCKER_HUB%/pd-backend:%BUILD_NUMBER%
                docker push %DOCKER_HUB%/pd-backend:latest
                '''
            }
        }

        stage('Push Frontend Image') {
            steps {
                bat '''
                docker push %DOCKER_HUB%/pd-frontend:%BUILD_NUMBER%
                docker push %DOCKER_HUB%/pd-frontend:latest
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat '''
                kubectl apply -f k8s/
                '''
            }
        }

        stage('Update Backend Image') {
            steps {
                bat '''
                kubectl set image deployment/pd-backend pd-backend=nithinp004/pd-backend:%BUILD_NUMBER%
                '''
            }
        }

        stage('Update Frontend Image') {
            steps {
                bat '''
                kubectl set image deployment/pd-frontend pd-frontend=nithinp004/pd-frontend:%BUILD_NUMBER%
                '''
            }
        }
    }
}
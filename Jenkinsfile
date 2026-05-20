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
                    sh '''
                    docker build \
                    -t $DOCKER_HUB/pd-backend:$BUILD_NUMBER \
                    -t $DOCKER_HUB/pd-backend:latest .
                    '''
                }
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                dir('frontend') {
                    sh '''
                    docker build \
                    -t $DOCKER_HUB/pd-frontend:$BUILD_NUMBER \
                    -t $DOCKER_HUB/pd-frontend:latest .
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

                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Backend Image') {
            steps {
                sh '''
                docker push $DOCKER_HUB/pd-backend:$BUILD_NUMBER
                docker push $DOCKER_HUB/pd-backend:latest
                '''
            }
        }

        stage('Push Frontend Image') {
            steps {
                sh '''
                docker push $DOCKER_HUB/pd-frontend:$BUILD_NUMBER
                docker push $DOCKER_HUB/pd-frontend:latest
                '''
            }
        }

stage('Update Backend Image') {
    steps {
        sh """
        kubectl set image deployment/pd-backend \
        pd-backend=nithinp004/pd-backend:${BUILD_NUMBER}
        """
    }
}

stage('Update Frontend Image') {
    steps {
        sh """
        kubectl set image deployment/pd-frontend \
        pd-frontend=nithinp004/pd-frontend:${BUILD_NUMBER}
        """
    }
}

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/ --validate=false'
            }
        }
    }
}

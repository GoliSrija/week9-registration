
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'golisrija123/week8:week9'
    }

    stages {
        stage('Build') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat 'echo %DOCKER_PASS% | docker login --username %DOCKER_USER% --password-stdin'
                    bat 'docker push %IMAGE_NAME%'
                    bat 'docker logout'
                }
            }
        }

        stage('Deploy') {
            steps {
                bat 'kubectl apply -f deployment.yaml'
                bat 'kubectl apply -f service.yaml'
                bat 'kubectl rollout status deployment/registration-app --timeout=180s'
            }
        }
    }
}

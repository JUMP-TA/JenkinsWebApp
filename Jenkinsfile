pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/JUMP-TA/JenkinsWebApp.git'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    echo "Logging into Docker Hub"
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKER_PWD', usernameVariable: 'DOCKER_USER')]) {
                        bat """
                            docker login -u %DOCKER_USER% -p %DOCKER_PWD%
                        """
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo "Building Docker image"
                    bat 'docker build -t seanbryson/jenkinswebapp:%BUILD_ID% .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    echo "Pushing Docker image to Docker Hub"
                    bat 'docker push seanbryson/jenkinswebapp:%BUILD_ID%'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying Docker image"
                    bat 'docker pull seanbryson/jenkinswebapp:%BUILD_ID%'
                    bat 'docker run -d -p 3000:3000 seanbryson/jenkinswebapp:%BUILD_ID%'
                }
            }
        }
    }

    post {
        always {
            echo "Cleaning workspace"
            cleanWs()
        }
    }
}

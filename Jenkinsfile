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

        stage('Build') {
            steps {
                script {
                    echo "Building Docker image"
                    def app = docker.build("seanbryson/jenkinswebapp:${env.BUILD_ID}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    echo "Logging into Docker Hub"
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKER_PWD', usernameVariable: 'DOCKER_USER')]) {
                        echo "Docker Username: ${env.DOCKER_USER}"
                        echo "Docker Password: ${env.DOCKER_PWD}"  // Debugging purposes, remove after verifying

                        bat """
                            echo %DOCKER_PWD% | docker login -u %DOCKER_USER% --password-stdin
                        """
                    }

                    echo "Pushing Docker image to Docker Hub"
                    docker.withRegistry('https://registry.hub.docker.com', 'DOCKERHUB_CREDENTIALS') {
                        def app = docker.image("seanbryson/jenkinswebapp:${env.BUILD_ID}")
                        app.push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying Docker image"
                    bat "docker pull seanbryson/jenkinswebapp:${env.BUILD_ID}"
                    bat "docker run -d -p 3000:3000 seanbryson/jenkinswebapp:${env.BUILD_ID}"
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



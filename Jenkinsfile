pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/JUMP-TA/JenkinsWebApp.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    def app = docker.build("seanbryson/jenkinswebapp:${env.BUILD_ID}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
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
                    sh 'docker pull seanbryson/jenkinswebapp:${env.BUILD_ID}'
                    sh 'docker run -d -p 3000:3000 seanbryson/jenkinswebapp:${env.BUILD_ID}'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
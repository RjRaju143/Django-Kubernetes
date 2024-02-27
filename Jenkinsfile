pipeline {
    agent any
    environment {
        DOCKER_USERNAME = 'rjraju'
        DOCKER_PASSWORD = credentials('dockerhub_credentials')
    }
    stages {        
        stage('Build Docker Image') {
            steps {
                echo "BUILD_ID: ${env.BUILD_ID}"
                dir('web') {
                    script {
                        def dockerTag = "${DOCKER_USERNAME}/django-k8s-web:latest"
                        docker.build(dockerTag, '-f Dockerfile .')
                    }
                }
            }
        }
        
        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    def dockerTag = "${DOCKER_USERNAME}/django-k8s-web:latest"
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_USERNAME, DOCKER_PASSWORD) {
                        docker.image(dockerTag).push()
                    }
                }
            }
        }
    }
}

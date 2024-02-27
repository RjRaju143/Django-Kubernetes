pipeline {
    agent any
    environment {
        DOCKER_USERNAME = 'rjraju'
        DOCKER_PASSWORD = credentials('dockerhub_credentials')
    }
    stages {        
        stage('Build and Push Image') {
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
        stage('Logs') {
            steps {
                sh 'docker push rjraju/django-k8s-web:latest'
                echo 'Done...'
            }
        }
    }
}

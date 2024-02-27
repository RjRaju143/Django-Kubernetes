pipeline {
    agent any
    
    environment {
        DOCKER_USERNAME = 'rjraju'
        DOCKER_PASSWORD = credentials('dockerhub_credentials')
    }
    
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    def GIT_COMMIT_ID = sh(returnStdout: true, script: 'git log -1 --pretty=format:%H | cut -c1-7').trim()
                    echo "Git Commit ID: ${GIT_COMMIT_ID}"
                    dir('web') {
                        def dockerTagLatest = "${DOCKER_USERNAME}/django-k8s-web:latest"
                        docker.build(dockerTagLatest, '-f Dockerfile .')
                        def dockerTagWithCommitID = "${DOCKER_USERNAME}/django-k8s-web:${GIT_COMMIT_ID}"
                        docker.build(dockerTagWithCommitID, '-f Dockerfile .')
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



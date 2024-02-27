pipeline {
    agent any
    
    environment {
        DOCKER_USERNAME = 'rjraju'
        DOCKER_PASSWORD = credentials('dockerhub_credentials')
    }
    
    stages {
        stage('Print Environment Variables') {
            steps {
                script {
                    // Echo Docker-related environment variables
                    echo "DOCKER_USERNAME: ${env.DOCKER_USERNAME}"
                    echo "DOCKER_PASSWORD: ${env.DOCKER_PASSWORD}"
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Get the Git commit ID
                    def GIT_COMMIT_ID = sh(returnStdout: true, script: 'git log -1 --pretty=format:%H | cut -c1-7').trim()
                    echo "Git Commit ID: ${GIT_COMMIT_ID}"
                    
                    // Build Docker image
                    dir('web') {
                        // Define Docker image tags
                        def dockerTagLatest = "${DOCKER_USERNAME}/django-k8s-web:latest"
                        def dockerTagWithCommitID = "${DOCKER_USERNAME}/django-k8s-web:${GIT_COMMIT_ID}"
                        
                        // Build Docker images
                        docker.build(dockerTagLatest, '-f Dockerfile .')
                        docker.build(dockerTagWithCommitID, '-f Dockerfile .')
                    }
                }
            }
        }
    }
}

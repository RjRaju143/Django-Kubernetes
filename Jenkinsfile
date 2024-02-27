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
                    // Get the Git commit ID
                    def GIT_COMMIT_ID = sh(returnStdout: true, script: 'git log -1 --pretty=format:%H | cut -c1-7').trim()

                    // Display the Git commit ID
                    echo "Git Commit ID: ${GIT_COMMIT_ID}"

                    // Build Docker image
                    dir('web') {
                        // Build Docker image with latest tag
                        def dockerTagLatest = "${DOCKER_USERNAME}/django-k8s-web:latest"
                        docker.build(dockerTagLatest, '-f Dockerfile .')

                        // Build Docker image with commit ID tag
                        def dockerTagWithCommitID = "${DOCKER_USERNAME}/django-k8s-web:${GIT_COMMIT_ID}"
                        docker.build(dockerTagWithCommitID, '-f Dockerfile .')
                    }
                }
            }
        }
    }
}

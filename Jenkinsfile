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

        stage('Push Docker Image to Docker Hub') {
            steps {
                // Push both latest and Git commit ID tagged images
                script {
                    // Extract Docker credentials from the secret text
                    def dockerCredentials = credentials('dockerhub_credentials')
                    def dockerAuth = dockerCredentials.split(':')
                    def dockerUsername = dockerAuth[0]
                    def dockerPassword = dockerAuth[1]
                    
                    // Log in to Docker Hub and push the images
                    sh "docker login -u ${dockerUsername} -p ${dockerPassword}"
                    sh "docker push ${DOCKER_USERNAME}/django-k8s-web:latest"
                    sh "docker push ${DOCKER_USERNAME}/django-k8s-web:${GIT_COMMIT_ID}"
                }
            }
        }
    }
}

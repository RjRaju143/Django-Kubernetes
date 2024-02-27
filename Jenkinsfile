pipeline {
    agent any
    environment {
        DOCKER_USERNAME = 'rjraju'
        DOCKER_PASSWORD = credentials('dockerhub_credentials')
        IMAGE_NAME = "django-k8s-web"
        GIT_COMMIT_ID = sh(returnStdout: true, script: 'git log -1 --pretty=format:%H | cut -c1-7').trim()
    }
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    dir('web') {
                        def dockerTagLatest = "${DOCKER_USERNAME}/${IMAGE_NAME}:latest"
                        def dockerTagWithCommitID = "${DOCKER_USERNAME}/${IMAGE_NAME}:${env.GIT_COMMIT_ID}"
                        docker.build(dockerTagLatest, '-f Dockerfile .')
                        docker.build(dockerTagWithCommitID, '-f Dockerfile .')
                    }
                }
            }
        }
        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                        sh "docker push ${DOCKER_USERNAME}/${IMAGE_NAME}:latest"
                        sh "docker push ${DOCKER_USERNAME}/${IMAGE_NAME}:${env.GIT_COMMIT_ID}"
                    }
                }
            }
        }
    }
}

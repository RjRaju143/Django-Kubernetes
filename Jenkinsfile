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
                    echo "DOCKER_USERNAME: ${env.DOCKER_USERNAME}"
                    echo "DOCKER_PASSWORD: ${env.DOCKER_PASSWORD}"
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def GIT_COMMIT_ID = sh(returnStdout: true, script: 'git log -1 --pretty=format:%H | cut -c1-7').trim()
                    echo "Git Commit ID: ${GIT_COMMIT_ID}"
                    dir('web') {
                        def dockerTagLatest = "${DOCKER_USERNAME}/django-k8s-web:latest"
                        def dockerTagWithCommitID = "${DOCKER_USERNAME}/django-k8s-web:${GIT_COMMIT_ID}"
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
                        sh "docker push ${DOCKER_USERNAME}/django-k8s-web:latest"
                        sh "docker push ${DOCKER_USERNAME}/django-k8s-web:${GIT_COMMIT_ID}"
                    }
                }
            }
        }
    }
}



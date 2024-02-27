pipeline {
    agent any
    
    environment {
        DOCKER_USERNAME = 'rjraju'
        DOCKER_PASSWORD = credentials('dockerhub_credentials')
    }
    stages {
        stage('Debug') {
            steps {
                script {
                    echo "GITHUB_SHA: ${env.GITHUB_SHA}"
                    echo "BUILD_ID: ${env.BUILD_ID}"
                }
            }
        }
        
        stage('Build and Push Image') {
            steps {
                dir('web') {
                    script {
                        def dockerTag = "${DOCKER_USERNAME}/django-k8s-web:latest"
                        // def dockerTagWithSHA = "${DOCKER_USERNAME}/django-k8s-web:${env.GITHUB_SHA.take(7)}-${env.BUILD_ID.take(5)}"

                        docker.build(dockerTag, '-f Dockerfile .')
                        // docker.build(dockerTagWithSHA, '-f Dockerfile .')
                        docker.withRegistry('https://index.docker.io/v1/', DOCKER_USERNAME, 'dckr_pat_pMBbjUXNCqQzQggKYpk7v6PRQjg') {
                            docker.push(dockerTag)
                            // docker.push(dockerTagWithSHA)
                        }
                    }
                }
            }
        }
    }
}

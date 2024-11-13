pipeline {
    agent any
    environment {
        REGISTRY="docker.io"
        dockerRegistryCredential='docker-hub-harun' // Ensure this matches the ID created in Jenkins
        dockerImage = ''
        DOCKER_REGISTRY_URL="https://$REGISTRY"
        IMAGE_CREATED_BY="jenkins"
        PROJECT_NAME="php-app-prod"
        GIT_TAG=sh(returnStdout: true, script: '''
            echo $(git describe --tags)
        ''').trim()
        IMAGE_VERSION="$BUILD_NUMBER-$IMAGE_CREATED_BY"
        DOCKER_TAG="$REGISTRY/$PROJECT_NAME:$IMAGE_VERSION"
        DISCORD_WEBHOOK_URL = 'https://discord.com/api/webhooks/your-webhook-url'
        AUTHOR_NAME="Harunur Rashid"
        AUTHOR_EMAIL="harunur@ba-systems.com"
    }

    stages {
        stage('Building Docker image') {
            steps {
                script {
                    dockerImage = docker.build("$DOCKER_TAG", "-f ./Dockerfile .")
                }
            }
        }

        stage('Push Docker image') {
            steps {
                script {
                    docker.withRegistry("$DOCKER_REGISTRY_URL", dockerRegistryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}

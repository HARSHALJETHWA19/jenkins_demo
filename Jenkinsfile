pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'dockerhub' // Replace with your registry name
        DOCKER_CRED_ID = 'docker-hub-credentials' // ID of your Docker Hub credentials in Jenkins
        IMAGE_NAME = 'my-node-app:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub repository
                git branch: 'main', url: 'https://github.com/HARSHALJETHWA19/jenkins_demo'
            }
        }
        stage('Build') {
            steps {
                script {
                    docker.withRegistry('https://' + env.DOCKER_REGISTRY, env.DOCKER_CRED_ID) {
                        // Build and push the Docker image
                        docker.build(env.IMAGE_NAME, '.').push()
                    }
                }
            }
        }
        stage('Test') {
            steps {
                // Add test commands here
                sh 'echo "Running tests"'
            }
        }
        stage('Deploy') {
            steps {
                // Pull the Docker image and run the container
                script {
                    docker.withRegistry('', env.DOCKER_CRED_ID) {
                        docker.image(env.IMAGE_NAME).pull()
                        docker.image(env.IMAGE_NAME).run('-p 3000:3000 -d')
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up: stop and remove the container
            script {
                docker.image(env.IMAGE_NAME).stop()
                docker.image(env.IMAGE_NAME).remove()
            }
        }
    }
}

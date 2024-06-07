pipeline {
    agent any
    
    environment {
        // Define Docker Hub credentials stored in Jenkins
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-creds')
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull the Git repository
                git branch: 'staging', url: 'https://github.com/anhnv-vietnam/examples.git'
            }
        }

        stage('Build') {
            steps {
                // Execute the build command to build the container image
                script {
                    sh '''
                    cd guestbook-go
                    make build
                    '''
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                // Login to Docker Hub and push the container image
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                        sh 'echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USERNAME --password-stdin'
                        sh '''
                        cd guestbook-go
                        make push
                        '''
                        sh 'docker logout'
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline executed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

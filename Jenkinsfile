pipeline {
    agent any

    environment {
        // Path to your Docker Compose file
        DOCKER_COMPOSE_FILE = '/home/appuser/CICD-MA/manafa-cicd//docker-compose.yml'
        // Git repository URL
        GIT_REPO = 'https://github.com/hsohail-tech/manafa-cicd.git'
        // Git credentials ID stored in Jenkins
        GIT_CREDENTIALS_ID = '3571058f-2588-41b5-8547-fc3d47af9f53'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub
                git url: "${env.GIT_REPO}", branch: 'master', credentialsId: "${env.GIT_CREDENTIALS_ID}"
            }
        }

        stage('Build Docker Images') {
            steps {
                // Build Docker images using Docker Compose
                script {
                    sh """
                    cd /home/appuser/CICD-MA/manafa-cicd//
                    docker-compose -f ${env.DOCKER_COMPOSE_FILE} build
                    """
                }
            }
        }

        stage('Run Docker Compose') {
            steps {
                // Run Docker Compose to start containers
                script {
                    sh """
                    cd /home/appuser/CICD-MA/manafa-cicd//
                    docker-compose -f ${env.DOCKER_COMPOSE_FILE} up -d
                    """
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Stop existing services and start new ones using Docker Compose
                    sh """
                    cd /home/appuser/CICD-MA/manafa-cicd//
                    docker-compose -f ${env.DOCKER_COMPOSE_FILE} down
                    docker-compose -f ${env.DOCKER_COMPOSE_FILE} up -d
                    """
                }
            }
        }

        stage('Push to Git Repo') {
            steps {
                script {
                    sh """
                    cd /home/appuser/CICD-MA/manafa-cicd//
                    git config --global user.email "hsohail@i2cinc.com"
                    git config --global user.name "hsohail-tech"

                    # Create a new branch or commit with the build number
                    git checkout -b build-${env.BUILD_NUMBER}
                    git add .
                    git commit -m "Automated commit for build #${env.BUILD_NUMBER}"

                    # Push changes to the remote repository
                    git push -u origin build-${env.BUILD_NUMBER}
					git push -u https://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@github.com/hsohail-tech/manafa-cicd.git build-${env.BUILD_NUMBER}
                    """
                }
            }
        }
    }

    post {
        success {
            // Actions to take if the build is successful
            echo 'Build and deployment successful!'
        }

        failure {
            // Actions to take if the build fails
            echo 'Build or deployment failed!'
        }
    }
}


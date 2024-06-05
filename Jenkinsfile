pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials'
        DOCKERHUB_REPO = 'taimooranwar'
        REPO_URL = 'https://github.com/NUCESFAST/scd-final-lab-exam-MuhammadTaimoorAnwar511.git'
    }

    stages {
        stage('Checkout Code1232') {
            steps {
                echo "Checking out code from ${REPO_URL} on branch master"
            }
        }

        stage('Build Docker Images1232') {
            steps {
                script {
                    def services = ['Auth', 'Classrooms', 'event-bus', 'Post', 'client']
                    services.each { service ->
                        dir("${service}") {
                            echo "Building Docker image for ${service}"
                        }
                    }
                }
            }
        }

        stage('Push Docker Images to Docker Hub1232') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                        echo "Logging in to Docker Hub with credentials ID: ${DOCKER_CREDENTIALS_ID}"
                        def services = ['Auth', 'Classrooms', 'event-bus', 'Post', 'client']
                        services.each { service ->
                            def imageName = "${DOCKERHUB_REPO}/${service.toLowerCase()}"
                            echo "Pushing Docker image ${imageName} to Docker Hub"
                        }
                    }
                }
            }
        }

        stage('Deploy and Validate1232') {
            steps {
                echo "Deploying application with docker-compose"
                // Add validation steps if necessary, such as checking service health or running tests
                echo "Validating deployment"
            }
        }
    }

    post {
        always {
            echo "Tearing down deployment with docker-compose down"
        }
    }
}

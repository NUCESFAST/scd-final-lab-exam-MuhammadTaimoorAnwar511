pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials'
        DOCKERHUB_REPO = 'taimooranwar'
        REPO_URL = 'https://github.com/NUCESFAST/scd-final-lab-exam-MuhammadTaimoorAnwar511.git'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: "${REPO_URL}", branch: 'master'
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    def services = ['Auth', 'Classrooms', 'event-bus', 'Post', 'client']
                    services.each { service ->
                        dir("${service}") {
                            bat 'npm install'  // Use 'bat' for Windows equivalent of 'sh'
                            def imageName = "${DOCKERHUB_REPO}/${service.toLowerCase()}"
                            bat "docker build -t ${imageName} ."
                        }
                    }
                }
            }
        }

        stage('Push Docker Images to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                        def services = ['Auth', 'Classrooms', 'event-bus', 'Post', 'client']
                        services.each { service ->
                            def imageName = "${DOCKERHUB_REPO}/${service.toLowerCase()}"
                            bat "echo ${DOCKERHUB_PASSWORD} | docker login -u ${DOCKERHUB_USERNAME} --password-stdin"
                            bat "docker push ${imageName}"
                        }
                    }
                }
            }
        }

        stage('Deploy and Validate') {
            steps {
                bat 'docker-compose up -d'
                // Add validation steps if necessary, such as checking service health or running tests
            }
        }
    }

    post {
        always {
            bat 'docker-compose down'
        }
    }
}

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
                git url: "${REPO_URL}", branch: 'master'
            }
        }

        stage('Build Docker Images1232') {
            steps {
                script {
                    def services = ['Auth', 'Classrooms', 'event-bus', 'Post', 'client']
                    services.each { service ->
                        dir("${service}") {
                            sh 'npm install'
                            def imageName = "${DOCKERHUB_REPO}/${service.toLowerCase()}"
                            sh "docker build -t ${imageName} ."
                        }
                    }
                }
            }
        }

        stage('Push Docker Images to Docker Hub1232') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                        def services = ['Auth', 'Classrooms', 'event-bus', 'Post', 'client']
                        sh "echo ${DOCKERHUB_PASSWORD} | docker login -u ${DOCKERHUB_USERNAME} --password-stdin"
                        services.each { service ->
                            def imageName = "${DOCKERHUB_REPO}/${service.toLowerCase()}"
                            sh "docker push ${imageName}"
                        }
                    }
                }
            }
        }

        stage('Deploy and Validate1232') {
            steps {
                sh 'docker-compose up -d'
                
            }
        }
    }

    post {
        always {
            sh 'docker-compose down'
        }
    }
}

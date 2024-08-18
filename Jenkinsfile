pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "vikrant92498/your-app:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the Git repository
                git branch: 'main', url: 'https://github.com/Vikrant92498/sample_project.git'
            }
        }

        stage('Build with Maven') {
            steps {
                // Run Maven build
                withMaven(maven: 'Maven') {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build a Docker image
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                // Push Docker image to Docker Hub (requires Docker Hub login)
                script {
                    docker.withRegistry('', 'docker-hub-credentials') {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                // Deploy the application (e.g., run the Docker container)
                // This could be customized to your deployment environment
                sh "docker run -d -p 8080:8080 ${DOCKER_IMAGE}"
            }
        }
    }

    post {
        always {
            // Clean up Docker containers and images
            sh 'docker system prune -f'
        }
    }
}

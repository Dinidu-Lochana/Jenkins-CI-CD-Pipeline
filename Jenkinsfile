pipeline {
    agent any // Define the environment where the pipeline runs

    stages {
        stage('SCM Checkout') { // Clone the Git repository
            steps {
                retry(3) { // Retry the checkout step up to 3 times if it fails
                    git branch: 'main', url: 'https://github.com/Dinidu-Lochana/Jenkins-CI-CD-Pipeline'
                }
            }
        }
        stage('Build Docker Image') { // Build the Docker image
            steps {
                bat 'docker build -t 24dinidulochana/nodeapp-test:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') { // Log in to Docker Hub
            steps {
                withCredentials([string(credentialsId: 'test-dockerpassword', variable: 'DOCKER_PASSWORD')]) {
                    script {
                        bat "docker login -u 24dinidulochana -p %DOCKER_PASSWORD%"
                    }
                }
            }
        }
        stage('Push Image') { // Push the Docker image to Docker Hub
            steps {
                bat 'docker push 24dinidulochana/nodeapp-test:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always { // This block is executed regardless of the build result
            bat 'docker logout'
        }
    }
}

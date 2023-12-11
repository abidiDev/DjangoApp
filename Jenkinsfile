pipeline {
    agent {
        label 'agent-vagrant'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo 'SCM checkout successful!'
            }
        }

       stage('Lint Dockerfile') {
            steps {
                script {
                    // Run hadolint on the Dockerfile
                    sh 'docker run --rm -i hadolint/hadolint < Dockerfile'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh 'sudo docker build -t django_app_image .'
                }
            }
        }
    }

    post {
        success {
            echo 'Build successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}

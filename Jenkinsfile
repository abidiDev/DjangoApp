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

        stage('Build') {
            steps {
                // Install dependencies
                sh 'pip install -r requirements.txt'

                // Apply migrations
                sh 'python3 manage.py migrate'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh 'sudo docker buildx create --use'
                    sh 'sudo docker buildx build -t django_app_image --platform local .'

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

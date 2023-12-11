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

                sh 'python manage.py collectstatic'
'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
               

                    // Build Docker image
                    sh 'sudo docker  build -t django_app_image .'
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

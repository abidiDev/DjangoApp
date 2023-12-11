pipeline {
    agent {
        label 'agent-vagrant'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo 'scm successful!'
            }
        }
           stage('Build') {
            steps {
                // Install dependencies
                sh 'pip install -r requirements.txt'

                // Apply migrations
                sh 'python3 manage.py migrate'

                // Collect static files
                sh 'python3 manage.py collectstatic --noinput'
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

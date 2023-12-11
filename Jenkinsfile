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

                
            }
        }

          stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh 'docker build -t DjangoAppImage .'
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

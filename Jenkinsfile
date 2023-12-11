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
                    def lintResult = sh(script: 'sudo docker run --rm -i hadolint/hadolint', returnStatus: true)
                    
                    if (lintResult == 0) {
                        echo 'Linting successful, no issues found.'
                    } else {
                        echo 'Linting completed with warnings. Check the warnings above.'
                        currentBuild.result = 'UNSTABLE'
                    }
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
        
        stage('Run with Docker Compose') {
            steps {
                script {
                    
                    // Use Docker Compose to start the application
                    sh 'sudo docker-compose up -d'
                }
            }
        }

        /* stage('Push to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub
                    sh 'sudo docker login -u abidi123456 -p Bouselm852!'
                    
                    // Tag the built image with the Docker Hub repository name
                    sh 'sudo docker tag django_app_image abidi123456/django_app_image:latest'
                    
                    // Push the image to Docker Hub
                    sh 'sudo docker push abidi123456/django_app_image:latest'
                }
            }
        }
        */
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

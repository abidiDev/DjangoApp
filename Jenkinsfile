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

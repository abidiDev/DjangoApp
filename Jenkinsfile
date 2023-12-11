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





    post {
        success {
            echo 'Build successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
}

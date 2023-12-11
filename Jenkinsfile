pipeline {
    agent {
            label 'agent-vagrant'
        }

    environment {
            DOCKER_IMAGE = 'skimage'
            NEXUS_URL = 'http://192.168.33.12:8081'
            NEXUS_REPO = 'repository/maven-snapshots'
            GROUP_ID = 'tn.esprit.ds'
            ARTIFACT_ID = 'SkiStationProject'
            VERSION = '0.0.1-SNAPSHOT'
            NEXUS_DOCKER_REPO = 'repository/docker-releases' // Assurez-vous d'ajuster cela en fonction de votre configuration Nexus
            DOCKER_REGISTRY_URL = "${NEXUS_URL}/${NEXUS_DOCKER_REPO}"
        }

    tools {
        maven 'M2_HOME'
    }

    stages {


        stage('Checkout') {
            steps {
                checkout scm
            }
        }




        stage('Build') {
            steps {
                script {
                    // Use 'mvn' from the agent's machine
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Unit Tests') {
                    steps {
                        // Run unit tests
                        sh 'mvn test'
                    }
                }

       stage('SonarQube Analysis') {
                   steps {
                       script {
                           // Run SonarQube analysis using Maven
                           sh " mvn sonar:sonar -Dsonar.projectKey=devop1project -Dsonar.host.url=http://192.168.33.12:9000 -Dsonar.login=02168fd6e5ca7ff9d3c5a8eb5f730df19ed634d6"
                       }
                   }
               }



        stage('Deploy to Nexus') {
            steps {
                script {
                    // Deploy to Nexus
                    sh 'mvn deploy -X '
                }
            }
        }

        stage('Build Docker Image') {
                                    steps {
                                        script {
                                            def jarFileName = "${ARTIFACT_ID}-${VERSION}.jar"
                                            def nexusJarUrl = "${NEXUS_URL}/${NEXUS_REPO}/${GROUP_ID}/${ARTIFACT_ID}/${VERSION}/${jarFileName}"

                                            // Download the JAR file from Nexus
                                            sh "curl -O ${nexusJarUrl}"

                                            // Build Docker image
                                            sh " sudo docker build -t ${DOCKER_IMAGE} ."

                                            // Cleanup - remove the downloaded JAR file
                                            sh "rm ${jarFileName}"
                                        }
                                    }
                                }

                               stage('Push Docker Image to Nexus') {
                                   steps {
                                       script {
                                           // Tag Docker image with Nexus repository URL
                                           def nexusDockerImage = "${DOCKER_REGISTRY_URL}/${DOCKER_IMAGE}"
                                           sh "docker tag ${DOCKER_IMAGE} ${nexusDockerImage}"

                                           // Login to Nexus Docker registry
                                           sh "docker login -u your_nexus_username -p your_nexus_password ${NEXUS_URL}"

                                           // Push Docker image to Nexus repository
                                           sh "docker push ${nexusDockerImage}"

                                           // Logout from Nexus Docker registry
                                           sh "docker logout ${NEXUS_URL}"
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

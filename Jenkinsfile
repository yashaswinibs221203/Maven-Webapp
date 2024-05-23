pipeline {
    agent any

    environment {
        REPO_URL = 'REPO-URL'
        BRANCH = 'main'
        WAR_NAME = 'webapp.war'
        WAR_SOURCE_PATH = "${env.WORKSPACE}/webapp/target/${WAR_NAME}"
        TOMCAT_WEBAPP_DIR = '/home/ubuntu/apache-tomcat-10.1.24/webapps'
    }
    tools{
        maven 'MAVEN_HOME'
    }
    stages {
        stage('Clone Repository') {
            steps {
                // Clone the main branch of the repository from GitHub
                git branch: "${BRANCH}", url: "${REPO_URL}"
                sh 'whoami'
            }
        }
        stage('Build WAR') {
            steps {
                script {
                    // Build the project and package it as a WAR file using Maven
                    sh 'mvn clean package'
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    // Ensure the WAR file exists
                    if (!fileExists(WAR_SOURCE_PATH)) {
                        error "WAR file not found: ${WAR_SOURCE_PATH}"
                    }
                    
                    // Copy the WAR file to the Tomcat webapps directory
                    sh """
                    sudo cp ${WAR_SOURCE_PATH} ${TOMCAT_WEBAPP_DIR}
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment succeeded!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}

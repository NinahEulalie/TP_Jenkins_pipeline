pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([
                    string(
                    credentialsId: 'sonar-token',
                    variable: 'SONAR_TOKEN'
                    )
                ]) {

                    bat """
                    mvn sonar:sonar ^
                    -Dsonar.host.url=http://localhost:9000 ^
                    -Dsonar.token=%SONAR_TOKEN% ^
                    -Dsonar.projectKey=hello-jenkins
                    """
                }
            }
        }

        stage('Deploy to Nexus') {
            steps {
                bat 'mvn deploy'
            }
        }
    }
}
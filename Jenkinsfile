pipeline {
    agent any

    tools {
        maven 'Maven-3.9.6' // your Maven installation name in Jenkins
        jdk 'JDK-21'        // your JDK installation name in Jenkins
    }

    environment {
        SONAR_PROJECT_KEY = 'my-project'
        SONAR_ORGANIZATION = 'my-org'
        SONAR_PROJECT_NAME = 'simple-spring-api'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building with Maven...'
                bat 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube...'
                bat """
                    mvn sonar:sonar ^
                    -Dsonar.projectKey=%SONAR_PROJECT_KEY% ^
                    -Dsonar.organization=%SONAR_ORGANIZATION% ^
                    -Dsonar.projectName=%SONAR_PROJECT_NAME%
                """
            }
        }
    }

    post {
        always {
            echo "Pipeline finished with status: ${currentBuild.currentResult}"
        }
    }
}

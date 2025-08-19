pipeline {
    agent any

    // Tools configured in Jenkins
    tools {
        maven 'Maven-3.8.4'  // Make sure this matches your Jenkins Maven installation name
        jdk 'jdk-21'          // Make sure this matches your Jenkins JDK installation name
    }

    environment {
        MAVEN_OPTS = "-Dmaven.repo.local=$WORKSPACE/.m2/repository"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/RajuNadapana/simple-spring-api.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    // Create local Maven repository folder
                    sh 'mkdir -p $WORKSPACE/.m2/repository'

                    // Build the project with caching
                    sh 'mvn clean install -B -Dmaven.repo.local=$WORKSPACE/.m2/repository'
                }
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test -B -Dmaven.repo.local=$WORKSPACE/.m2/repository'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -B -Dmaven.repo.local=$WORKSPACE/.m2/repository'
            }
        }
    }

    post {
        always {
            echo "Cleaning up workspace"
            cleanWs()
        }
        success {
            echo "Build Successful!"
        }
        failure {
            echo "Build Failed!"
        }
    }
}

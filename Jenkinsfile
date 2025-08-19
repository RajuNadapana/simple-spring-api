pipeline {
    agent any

    // Tools configured in Jenkins
    tools {
        maven 'Maven-3.8.4'  // Make sure this matches your Jenkins Maven installation
        jdk 'JDK-21'         // Exact name from Jenkins (case-sensitive)
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
                    if (isUnix()) {
                        sh 'mkdir -p $WORKSPACE/.m2/repository'
                    } else {
                        bat 'mkdir "%WORKSPACE%\\.m2\\repository"'
                    }

                    // Build the project with caching
                    if (isUnix()) {
                        sh 'mvn clean install -B -Dmaven.repo.local=$WORKSPACE/.m2/repository'
                    } else {
                        bat 'mvn clean install -B -Dmaven.repo.local=%WORKSPACE%\\.m2\\repository'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn test -B -Dmaven.repo.local=$WORKSPACE/.m2/repository'
                    } else {
                        bat 'mvn test -B -Dmaven.repo.local=%WORKSPACE%\\.m2\\repository'
                    }
                }
            }
        }

        stage('Package') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn package -B -Dmaven.repo.local=$WORKSPACE/.m2/repository'
                    } else {
                        bat 'mvn package -B -Dmaven.repo.local=%WORKSPACE%\\.m2\\repository'
                    }
                }
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

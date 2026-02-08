pipeline {
    agent any
    triggers {
        githubPush()  // Automatically trigger on GitHub push events
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm  // Checkout the code from GitHub
            }
        }

        stage('Build') {
            when {
                branch 'develop'  // Run this stage only for the develop branch
            }
            steps {
                sh 'mvn clean compile'  // Build the application using Maven
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'  // Run unit tests using Maven
            }
        }

        stage('Package') {
            when {
                anyOf {
                    branch 'release/*'  // For release branches
                    branch 'main'       // For the main branch
                }
            }
            steps {
                sh 'mvn clean package'  // Build the package
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}

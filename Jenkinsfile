pipeline {
    agent any

    tools {
        // Defines the Maven tool version managed in your Jenkins Global Tool Configuration
        maven 'Maven 3.x' 
    }

    stages {
        stage('Checkout') {
            steps {
                // Clones the source code from your specific GitHub repository
                git branch: 'main', url: 'https://github.com/chiraggowda0316/ABC-Technologies.git'
            }
        }

        stage('Build') {
            steps {
                // Cleans target directory and compiles the source code
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                // Executes unit tests
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                // Generates the final executable JAR file, skipping test re-runs
                sh 'mvn package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                // Builds the Docker image from the local Dockerfile
                sh 'docker build -t abc-technologies:latest .'
            }
        }
    }
}


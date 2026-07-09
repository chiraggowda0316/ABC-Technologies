pipeline {
    agent any

    tools {
        // Must match the name configured in Jenkins -> Manage Jenkins -> Tools
        maven 'Maven 3.x' 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // 'SonarQube' must match the server name in Jenkins -> Manage Jenkins -> System
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t abc-technologies:latest .'
            }
        }

        stage('Docker Run') {
            steps {
                // Safely stops and removes old container before spinning up the new one
                sh '''
                    docker stop abc-app || true
                    docker rm abc-app || true
                    docker run -d --name abc-app -p 8080:8080 abc-technologies:latest
                '''
            }
        }
    }
}


pipeline {
    agent any

    tools {
        maven 'Maven 3.x' 
    }

    stages {
        stage('Checkout Source Code') {
            steps {
                // Delete any old files to keep the workspace clean
                cleanWs()
                
                // 1. CLONE THE DEVOPS INFRASTRUCTURE TEMPLATES (Dockerfile)
                dir('infra') {
                    git branch: 'main', url: 'https://github.com/chiraggowda0316/ABC-Technologies.git'
                }
                
                // 2. CLONE THE ACTUAL APPLICATION SOURCE CODE (pom.xml & src/)
                // REPLACE THE URL BELOW WITH THE ACTUAL SOURCE CODE REPOSITORY URL
                git branch: 'main', url: 'https://github.com'
                
                // 3. Move the Dockerfile out of the infra folder into the root workspace directory
                sh 'cp infra/Dockerfile .'
            }
        }

        stage('Build & Test') {
            steps {
                // This will now pass successfully because pom.xml exists in the root workspace
                sh 'mvn clean compile test'
            }
        }

        stage('Package') {
            steps {
                // Generates target/student.jar
                sh 'mvn package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                // This will now pass because target/student.jar and Dockerfile are in the same folder
                sh 'docker build -t abc-technologies:latest .'
            }
        }

        stage('Docker Run') {
            steps {
                sh '''
                    docker stop abc-app || true
                    docker rm abc-app || true
                    docker run -d --name abc-app -p 8080:8080 abc-technologies:latest
                '''
            }
        }
    }
}


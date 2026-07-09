pipeline {
    agent any

    tools {
        // Matches your registered Jenkins tool name environment hint
        maven 'Maven 3.x' 
    }

    stages {
        stage('Compile & Test') {
            steps {
                // Compiles project source files and runs unit tests
                sh 'mvn clean compile test'
            }
        }

        stage('Package App') {
            steps {
                // Packages application and renames target archive file for Dockerfile alignment
                sh '''
                    mvn package -DskipTests
                    mv target/*.jar target/student.jar
                '''
            }
        }

        stage('Docker Build') {
            steps {
                // Assembles the isolated application system image file locally
                sh 'docker build -t abc-technologies:latest .'
            }
        }

        stage('Docker Run') {
            steps {
                // Destroys conflicting resource constraints and spins up live server container
                sh '''
                    docker stop abc-app || true
                    docker rm abc-app || true
                    docker run -d --name abc-app -p 8080:8080 abc-technologies:latest
                '''
            }
        }
    }
}

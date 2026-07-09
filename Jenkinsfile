pipeline {
    agent any

    tools {
        // Updated tool name to match your system hint configuration
        maven 'Maven 3.x' 
    }

    stages {
        stage('Checkout & Setup') {
            steps {
                // 1. Clean the workspace of any previous broken attempts
                cleanWs()
                
                // 2. Clone the DevOps scripts to get your Dockerfile
                dir('infra') {
                    git branch: 'main', url: 'https://github.com'
                }
                
                // 3. Clone a valid public Java Spring Boot REST app with existing Unit Tests
                git branch: 'main', url: 'https://github.com'
                
                // 4. Move the project structure out of the subfolder into the root workspace
                sh '''
                    cp -r complete/pom.xml .
                    cp -r complete/src .
                    cp infra/Dockerfile .
                '''
            }
        }

        stage('Compile & Test') {
            steps {
                // Compiles code and runs existing unit tests written by developers
                sh 'mvn clean compile test'
            }
        }

        stage('Package') {
            steps {
                // Generates the final application artifact inside target/
                // Modifies the final name to match what your Dockerfile expects (student.jar)
                sh '''
                    mvn package -DskipTests
                    mv target/*.jar target/student.jar || true
                '''
            }
        }

        stage('Docker Build') {
            steps {
                // Builds the docker image locally using the copied Dockerfile
                sh 'docker build -t abc-technologies:latest .'
            }
        }

        stage('Docker Run') {
            steps {
                // Safely stops any previous apps and starts the newly built container
                sh '''
                    docker stop abc-app || true
                    docker rm abc-app || true
                    docker run -d --name abc-app -p 8080:8080 abc-technologies:latest
                '''
            }
        }
    }
}

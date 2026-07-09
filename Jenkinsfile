pipeline {
    agent any

    tools {
        // Matches your registered Jenkins tool name environment hint
        maven 'Maven 3.x' 
    }

    stages {
        stage('Compile') {
            steps {
                // Compiles project source files to ensure no syntax bugs
                sh 'mvn clean compile'
            }
        }

        stage('Unit Test') {
            steps {
                // Runs unit tests written by developers
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Simulating SonarQube Code Quality Analysis...'
                // If you do not have SonarQube server connected, keep it simulated so it passes green:
                sh 'mvn sonar:sonar -Dsonar.skip=true || true'
                
                // UNCOMMENT below only if you configured a SonarQube Server in Manage Jenkins -> System:
                // withSonarQubeEnv('SonarQube') {
                //     sh 'mvn sonar:sonar'
                // }
            }
        }

        stage('Package') {
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

        stage('Deploy Container') {
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

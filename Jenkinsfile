pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'mvn clean package'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                script {
                    // Running unit and integration tests
                    sh 'mvn test'
                }
            }
        }
        stage('Code Analysis') {
        steps {
            withSonarQubeEnv('SonarQube') {
                sh 'mvn clean verify sonar:sonar'
            }
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
            }
        }
    }
}

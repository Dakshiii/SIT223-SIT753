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
        stage('Security Scan') {
            steps {
                script {
                    // Using OWASP ZAP for security scanning
                    sh './run_security_scan.sh'
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

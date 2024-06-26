pipeline {
    agent any
    tools {
        maven 'Maven 3.6.3'
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building the application using Maven'
                script {
                    sh 'mvn clean package'
                }
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests using JUnit and Mockito'
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
                echo 'Performing security scan with OWASP ZAP'
                script {
                    sh './run_security_scan.sh'
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying application to AWS EC2 staging environment'
                script {
                    sh './deploy_staging.sh'
                }
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on the staging environment'
                script {
                    sh './test_staging.sh'
                }
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Deploying application to production environment'
                script {
                    sh './deploy_production.sh'
                }
            }
        }
    }
    post {
        success {
            script {
                withCredentials([
                    string(credentialsId: 'JENKINS_USER', variable: 'USER'),
                    string(credentialsId: 'JENKINS_TOKEN', variable: 'TOKEN')
                ]) {
                    def logFile = "${env.WORKSPACE}/console.log"
                    sh """
                        curl -u \$USER:\$TOKEN ${env.BUILD_URL}consoleText -o ${logFile}
                    """
                    emailext (
                        to: 'padni191@gmail.com',
                        subject: "SUCCESS: Pipeline completed successfully",
                        body: "The pipeline has completed successfully. Please find the console log attached.",
                        attachmentsPattern: '**/console.log'
                    )
                }
            }
        }
        failure {
            script {
                withCredentials([
                    string(credentialsId: 'JENKINS_USER', variable: 'USER'),
                    string(credentialsId: 'JENKINS_TOKEN', variable: 'TOKEN')
                ]) {
                    def logFile = "${env.WORKSPACE}/console.log"
                    sh """
                        curl -u \$USER:\$TOKEN ${env.BUILD_URL}consoleText -o ${logFile}
                    """
                    emailext (
                        to: 'padni191@gmail.com',
                        subject: "FAILURE: Pipeline failed",
                        body: "The pipeline has failed. Please find the console log attached.",
                        attachmentsPattern: '**/console.log'
                    )
                }
            }
        }
    }
}

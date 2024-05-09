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
                    // Run tests and generate test reports
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
                    // Example of running a security scan tool
                    sh '/home/darkfoxy/Documents/ass/run_security_scan.sh'
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
            mail to: 'd42kw01f@gmail.com',
                 subject: "SUCCESS: Pipeline completed successfully",
                 body: "The pipeline has completed successfully."
        }
        failure {
            mail to: 'd42kw01f@gmail.com',
                 subject: "FAILURE: Pipeline failed",
                 body: "The pipeline has failed. Please check the Jenkins logs for more details."
        }
    }
}

pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'SONARQUBE_SERVER' // Replace with your SonarQube server name in Jenkins
        SONAR_TOKEN = credentials('SONAR_TOKEN') // Use the ID you set in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                script {
                    // Build the Java project using Maven
                    sh 'mvn clean package'
                }
            }
        }
        
        stage('SonarQube Scan') {
            steps {
                script {
                    // Run SonarQube analysis
                    withSonarQubeEnv(SONARQUBE_SERVER) {
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }
        
        stage('Quality Gate') {
            steps {
                script {
                    // Wait for SonarQube's Quality Gate
                    timeout(time: 5, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution complete.'
        }
        success {
            echo 'Pipeline succeeded.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

pipeline {
    agent any

    tools {nodejs "node_18_19"}

    stages {
        stage('SonarQube Analysis') {
            environment {
                scannerHome = tool 'sonarqube-tool';
            }  
            steps {
                withSonarQubeEnv(credentialsId: 'sonarqube-secret',installationName: 'sonarqube-server' ) {
                      sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        stage("Quality gate") {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
        stage('Deploy Web') {
            steps {
                sh "docker compose up -d --build"
            }
        }
    }
}

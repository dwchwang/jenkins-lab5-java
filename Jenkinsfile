pipeline {
    agent {
        docker {
            image 'maven:3.9-eclipse-temurin-17'
        }
    }
    stages {
        stage('Build & Test') {
            steps {
                sh 'mvn -B clean verify'
            }
        }
    }
    post {
        always {
            junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
        }
        success {
            archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: true
        }
    }
}

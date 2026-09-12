pipeline {
    agent {
        docker {
            image 'maven:3.9-eclipse-temurin-17'    // dùng Docker agent luôn (sạch)
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
            junit 'target/surefire-reports/*.xml'    // publish test results
        }
        success {
            archiveArtifacts 'target/*.jar'
        }
    }
}

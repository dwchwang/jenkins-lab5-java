pipeline {
    agent {
        docker {
            image 'maven:3.9-eclipse-temurin-17'
        }
    }
    stages {
        stage('Quality & Build Checks') {
            parallel {
                stage('Unit Test') {
                    steps {
                        sh 'mvn test'
                    }
                }
                stage('Code Style Check') {
                    steps {
                        // Kiểm tra biên dịch code mà không cần chạy test lại
                        sh 'mvn compile'
                    }
                }
                stage('Package Check') {
                    steps {
                        // Đóng gói thử JAR file
                        sh 'mvn package -DskipTests'
                    }
                }
            }
        }
        stage('Deploy Staging') {
            when { 
                branch 'main' 
            }
            steps {
                echo 'Deploying Java App to Staging (Chỉ chạy trên nhánh main)...'
            }
        }
        stage('Deploy Production') {
            when {
                allOf {
                    branch 'main'
                    expression { return env.BUILD_NUMBER.toInteger() > 0 }
                }
            }
            steps {
                echo 'Deploying Java App to Production...'
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

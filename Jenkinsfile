pipeline {
    agent any
    environment {
        // LƯU Ý: Đổi 'username-cua-ban' thành tên tài khoản GitHub (viết chữ THƯỜNG)
        IMAGE = "ghcr.io/dwchwang/lab9-java"
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE:$BUILD_NUMBER -t $IMAGE:latest .'
            }
        }
        stage('Push Image to GHCR') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'ghcr-creds',
                    usernameVariable: 'REG_USER',
                    passwordVariable: 'REG_PASS'
                )]) {
                    sh '''
                        echo "$REG_PASS" | docker login ghcr.io -u "$REG_USER" --password-stdin
                        docker push $IMAGE:$BUILD_NUMBER
                        docker push $IMAGE:latest
                    '''
                }
            }
        }
        stage('Cleanup Local Images') {
            steps {
                sh 'docker rmi $IMAGE:$BUILD_NUMBER $IMAGE:latest || true'
            }
        }
    }
}

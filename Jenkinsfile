pipeline {
    agent {
        docker {
            image 'docker:20.10.7'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    environment {
        DOCKER_CONFIG = '/var/jenkins_home/.docker'
        DOCKER_BUILDKIT = '0'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/<username>/jenkins-demo-app.git'
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build -t jenkins-demo-app:latest .'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
                // รัน pytest ใน container ของ image ที่เพิ่ง build เสร็จ
                sh 'docker run --rm jenkins-demo-app:latest pytest || true'
            }
        }
        stage('Run Container') {
            steps {
                sh 'docker rm -f demo-app || true'
                sh 'docker run -d -p 8081:8081 --name demo-app jenkins-demo-app:latest'
            }
        }
    }
}

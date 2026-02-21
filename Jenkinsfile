pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp:latest .'
            }
        }

        stage('Show Docker Images') {
            steps {
                sh 'docker images'
            }
        }
    }
}

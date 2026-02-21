pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "manav7688/myapp"
        PROD_IP = "YOUR_PROD_PUBLIC_IP"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE:latest'
            }
        }

        stage('Deploy to Production') {
            steps {
                sshagent(['prod-ssh']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@$PROD_IP "
                        docker stop myapp || true &&
                        docker rm myapp || true &&
                        docker pull manav7688/myapp:latest &&
                        docker run -d -p 8081:80 --name myapp manav7688/myapp:latest
                    "
                    '''
                }
            }
        }
    }
}

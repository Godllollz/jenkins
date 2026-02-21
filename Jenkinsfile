stage('Deploy to Production') {
    steps {
        sshagent(['prod-ssh']) {
            sh '''
            ssh -o StrictHostKeyChecking=no ubuntu@YOUR_PROD_IP "
                docker stop myapp || true &&
                docker rm myapp || true &&
                docker pull manav7688/myapp:latest &&
                docker run -d -p 8081:80 --name myapp manav7688/myapp:latest
            "
            '''
        }
    }
}

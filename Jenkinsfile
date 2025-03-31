node {
    def app
    stage('Clone repository') {
        checkout scm
    }
    stage('Build image') {
       app = docker.build("gagal1818/kiii-lab4")
    }
    stage('Push image') {   
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
        }
    }
    // stage('Deploy') {
    //     sshagent(['my-ssh-credentials']) {
    //         sh """
    //             ssh root@your-server 'docker pull gagal1818/kiii-lab4:${env.BRANCH_NAME}-${env.BUILD_NUMBER}'
    //             ssh user@your-server 'docker stop my-container || true'  # Stop the existing container
    //             ssh user@your-server 'docker rm my-container || true'    # Remove the stopped container
    //             ssh user@your-server 'docker run -d --name my-container gagal1818/kiii-lab4:${env.BRANCH_NAME}-${env.BUILD_NUMBER}'
    //         """
    //     }
    // }
}

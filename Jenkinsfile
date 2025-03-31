pipeline {
    agent any

    environment {
        DEPLOY_SERVER = 'your-server'
    }

    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }

        stage('Build image') {
            steps {
                script {
                    app = docker.build("gagal1818/kiii-lab4")
                }
            }
        }

        stage('Push image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['my-ssh-credentials']) {
                    sh """
                    ssh root@${DEPLOY_SERVER} 'docker pull gagal1818/kiii-lab4:${env.BRANCH_NAME}-${env.BUILD_NUMBER}'
                    ssh root@${DEPLOY_SERVER} 'docker stop my-container || true'  # Stop the existing container
                    ssh root@${DEPLOY_SERVER} 'docker rm my-container || true'    # Remove the stopped container
                    ssh root@${DEPLOY_SERVER} 'docker run -d --name my-container gagal1818/kiii-lab4:${env.BRANCH_NAME}-${env.BUILD_NUMBER}'  # Run new container
                    """
                }
            }
        }
    }
}

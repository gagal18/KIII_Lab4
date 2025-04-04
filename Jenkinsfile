pipeline {
    agent any

    parameters {
        string(name: 'USER', defaultValue: '', description: 'Enter a value for IP')
        string(name: 'IP', defaultValue: '', description: 'Enter a value for IP')
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
                script{
                    sh """
                    ssh ${params.USER}@${params.IP} 'docker pull gagal1818/kiii-lab4:${env.BRANCH_NAME}-${env.BUILD_NUMBER}'
                    ssh ${params.USER}@${params.IP} 'docker stop my-container || true'  # Stop the existing container
                    ssh ${params.USER}@${params.IP} 'docker rm my-container || true'    # Remove the stopped container
                    ssh ${params.USER}@${params.IP} 'docker run -d --name my-container gagal1818/kiii-lab4:${env.BRANCH_NAME}-${env.BUILD_NUMBER}'  # Run new container
                    """
                }
            }
        }
    }
}

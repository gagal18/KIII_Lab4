pipeline {
    agent any
    triggers {
        githubPush()
    }
    environment {
        PORT = 8002
    }
    stages {
        stage('Set environment variables for branch') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'dev') {
                        env.PORT = 8001
                    }
                    echo "Current Branch: ${env.BRANCH_NAME}, Docker Image: ${env.DOCKER_IMAGE_NAME}"
                }
            }
        }
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }

        stage('Build image') {
            steps {
                script {
                    app = docker.build('gagal1818/kiii-lab4')
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
                script {
                    sh """
                    ssh root@${params.IP} 'docker pull gagal1818/kiii-lab4:${env.BRANCH_NAME}-${env.BUILD_NUMBER}'
                    ssh root@${params.IP} 'docker stop gagal1818_KIII_4${env.BRANCH_NAME} || true'
                    ssh root@${params.IP} 'docker rm gagal1818_KIII_4${env.BRANCH_NAME} || true'
                    ssh root@${params.IP} 'docker run -d -p 80:${env.PORT} --name gagal1818_KIII_4${env.BRANCH_NAME} gagal1818/kiii-lab4:${env.BRANCH_NAME}-${env.BUILD_NUMBER}'
                    """
                }
            }
        }
    }
}

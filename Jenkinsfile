pipeline {
    agent any
    triggers {
        githubPush()
    }
    environment {
        PORT = 8002
        USER = 'root'
        IP = '83.229.87.158'
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
        stage('Set environment variables for branch') {
            steps {
                script {
                    echo "${env.BRANCH_NAME}"
                    if (env.BRANCH_NAME == 'master') {
                        env.PORT = 8001
                    }
                    echo "Current Branch: ${env.BRANCH_NAME}, Docker Image: ${env.PORT}"
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh """
                    ssh ${env.USER}@${env.IP} 'docker pull gagal1818/kiii-lab4:${env.BRANCH_NAME}-${env.BUILD_NUMBER}'
                    ssh ${env.USER}@${env.IP} 'docker stop gagal1818_KIII_4${env.BRANCH_NAME} || true'
                    ssh ${env.USER}@${env.IP} 'docker rm gagal1818_KIII_4${env.BRANCH_NAME} || true'
                    ssh ${env.USER}@${env.IP} 'docker run -d -p ${env.PORT}:80 --name gagal1818_KIII_4${env.BRANCH_NAME} gagal1818/kiii-lab4:${env.BRANCH_NAME}-${env.BUILD_NUMBER}'
                    """
                }
            }
        }
    }
}

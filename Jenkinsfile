node {
    def app

    parameters {
        string(name: 'DEPLOY_USER', defaultValue: '', description: 'SSH username for deployment')
        string(name: 'DEPLOY_SERVER', defaultValue: '', description: 'IP address or hostname of the server')
        password(name: 'DEPLOY_SSH_PRIVATE_KEY', defaultValue: '', description: 'Private SSH key for deployment') // Optional if using key
    }
    
    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE    = 'sqlite'
    }
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
    stage('Deploy') {
        script {
            echo "Deploying with the following environment variables:"
            echo "SSH User: ${params.DEPLOY_USER}"
            echo "SSH Server: ${env.BUILD_ID}"
            sh "echo $ENV1" // prints default
            withEnv(['ENV1=newvalue']) {
                sh "echo $ENV1" // prints newvalue
            }
            echo "SSH Server: ${params.DEPLOY_SERVER}"
            echo "SSH Key (base64 encoded): ${params.DEPLOY_SSH_PRIVATE_KEY?.length() > 0 ? 'Provided' : 'Not Provided'}"
        }
    }
}

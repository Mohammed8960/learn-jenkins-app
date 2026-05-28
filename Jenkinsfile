pipeline {
    agent any

    environment {
        CI = 'true'
    }

    stages {
        stage('Prepare') {
            steps {
                script {
                    env.DOCKER_UID = sh(script: 'id -u', returnStdout: true).trim()
                    env.DOCKER_GID = sh(script: 'id -g', returnStdout: true).trim()
                }
            }
        }

        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args "-u ${env.DOCKER_UID}:${env.DOCKER_GID}"
                }
            }
            steps {
                sh '''
                    set -eux
                    node --version
                    npm --version
                    rm -rf node_modules
                    npm ci --no-audit --no-fund
                    npm run build
                '''
            }
        }
    }
}

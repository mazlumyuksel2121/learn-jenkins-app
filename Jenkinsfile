pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
         stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                echo 'Testing the Jenkins to perform Assignment...'
                   
                    cd C:\Users\mazlu\learn-jenkins-app\public
                    ls -la
                    grep "index.html" build/C:\Users\mazlu\learn-jenkins-app\public
                    npm test

                '''
            }
        }
    }
}

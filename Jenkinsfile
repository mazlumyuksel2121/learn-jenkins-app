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
                echo 'TEST STAGE'
                echo 'Testing the Jenkins to perform Assignment...'
                   
                    cd public
                    ls -la
                    test -f build/index.html && echo "index.html found" || echo "index.html not found"
                    npm test

                '''
            }
        }
                stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
//Once serve starts the server, it keeps running — forever — which means Jenkins can’t move to the next command.
         //Solution: Run serve in the background using &:  
         //Also, give it a few seconds to start up: with Sleep command
            steps {
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build &  
            
                    sleep 10
                    npx playwright test  --reporter=html
                '''
            }
        }
    }
    //Always runs after all stages — even if some fail.
//Publishes the JUnit test report from jest-results/junit.xml
//This is used to show test trends and results in Jenkins UI
    post {
        always {
                junit 'jest-results/junit.xml'
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
               }
        }

}

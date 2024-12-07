pipeline {
    agent any

    stages {
    /*
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
    */
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    #test -f build/index.html
                    npm test
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.49.0-noble'
                    reuseNode true
                }
            }

            steps {
                 sh '''
                     node --version
                     npm --version
                     npm install http-server
                     node_modules/.bin/http-server ./build -p 8080 &
                     sleep 10
                     npx playwright test
                 '''
             }
        }
    }

    post {
        always {
            junit 'jest-results/junit.xml'
        }
    }
}

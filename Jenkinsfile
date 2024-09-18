pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true  //  important to keep things strictly to only one workspace
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
                    reuseNode true  //  important to keep things strictly to only one workspace
                }
            }

            steps {
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true  //  important to keep things strictly to only one workspace
                }
            }

            steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}

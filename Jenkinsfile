pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '828d7d31-c7b1-4c85-82ee-78ffe5c16792'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')   //  'netlify-token' is the AUTHENTICATION TOKEN stored in Jenkins credentials
    }

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
                    echo "Deploying to Netlify Production Site. Site ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
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

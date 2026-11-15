pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '9dea2ecf-499e-4000-a8e2-0dae2d268be0'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

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
                    echo "===== Build Started ====="

                    echo "Listing project files:"
                    ls -la

                    echo "Node.js version:"
                    node --version

                    echo "NPM version:"
                    npm --version

                    echo "Installing dependencies:"
                    npm ci

                    echo "Building project:"
                    npm run build

                    echo "Listing files after build:"
                    ls -la

                    echo "===== Build Completed ====="
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
            environment {
                CI = 'true'
                JEST_JUNIT_OUTPUT_DIR = 'test-results'
                JEST_JUNIT_OUTPUT_NAME = 'junit.xml'
            }
            steps {
                sh '''
                    echo "===== Test Started ====="
                    npm test -- --watchAll=false --forceExit --reporters=default --reporters=jest-junit
                    echo "===== Test Completed ====="
                '''
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:20-alpine'
                    reuseNode true
                }
            }

            steps {
                    sh '''
                        echo "===== Deploy Started ====="
                        npm install netlify-cli@17 
                        node_modules/.bin/netlify --version
                        node_modules/.bin/netlify status
                        node_modules/.bin/netlify deploy --dir=build --prod
                        echo "===== Deploy Completed ====="
                '''
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true, testResults: 'test-results/junit.xml'
        }
    }
}


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

            steps{
                sh '''
                    echo "===== Test Started ====="

                    echo "Running tests:"
                    npm test

                    echo "===== Test Completed ====="
                '''
            }
        }

    }

    post {
        always {
            junit 'test-result/test-result.xml'
        }
    }
}

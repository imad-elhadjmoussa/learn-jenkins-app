
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

    }

    post {
    always {
        junit allowEmptyResults: true, testResults: 'test-results/junit.xml'
    }
}
}

pipeline {
    agent {
        docker {
            image 'node:16'  // Use Node 16 Docker image as the build agent
            args '-u root'  // Run as root to avoid permission issues
        }
    }

    environment {
        // Add any environment variables you need, for example, NPM_TOKEN for private packages
    }

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    // Install the project dependencies using npm
                    sh 'npm install --save'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Optionally, add a step to run tests if there are any defined in package.json
                    sh 'npm test'
                }
            }
        }

        stage('Snyk Security Scan') {
            steps {
                script {
                    // Install Snyk globally and run the security scan
                    sh 'npm install -g snyk'
                    // Authenticate Snyk if necessary (add your Snyk auth token to environment variables)
                    // sh 'snyk auth $SNYK_TOKEN'
                    // Run Snyk security scan
                    sh 'snyk test --severity-threshold=high'
                }
            }
        }
    }

    post {
        always {
            // Clean up resources if necessary
            echo 'Pipeline finished.'
        }

        failure {
            // Actions to take if the pipeline fails (notifications, etc.)
            echo 'Pipeline failed.'
        }
    }
}

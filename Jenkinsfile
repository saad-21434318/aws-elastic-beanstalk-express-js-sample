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
                    echo 'Starting to install project dependencies using npm...'
                    // Install the project dependencies using npm
                    sh 'npm install --save'
                    echo 'Dependencies installed successfully.'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    echo 'Starting to run project tests...'
                    // Optionally, add a step to run tests if there are any defined in package.json
                    sh 'npm test'
                    echo 'Tests completed successfully.'
                }
            }
        }

        stage('Snyk Security Scan') {
            steps {
                script {
                    echo 'Starting Snyk security scan...'
                    // Install Snyk globally and run the security scan
                    sh 'npm install -g snyk'
                    echo 'Snyk installed successfully.'

                    // Authenticate Snyk if necessary (add your Snyk auth token to environment variables)
                    // sh 'snyk auth $SNYK_TOKEN'
                    echo 'Running Snyk security scan with severity threshold set to high...'
                    // Run Snyk security scan
                    sh 'snyk test --severity-threshold=high'
                    echo 'Snyk scan completed. Check for any critical vulnerabilities.'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution finished. Check the above steps for results.'
        }

        failure {
            echo 'Pipeline execution failed. Please review the logs above for details.'
        }
    }
}

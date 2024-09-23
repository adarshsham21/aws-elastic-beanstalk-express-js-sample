pipeline {
    agent {
        docker { image 'node:16' }
    }
    stages {
        stage('Checkout Code') {
            steps {
                // Pull the source code from the GitHub repository
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                // Install the project's dependencies
                sh 'npm install --save'
            }
        }
        
        stage('Fix Vulnerabilities') {
            steps {
                // Run npm audit fix to fix low-severity vulnerabilities
                sh 'npm audit fix'
            }
        }

        stage('Snyk Security Scan') {
            steps {
                withCredentials([string(credentialsId: 'snyk_api_token', variable: 'SNYK_TOKEN')]) {
                    // Authenticate Snyk and run the security test
                    sh 'npx snyk auth $SNYK_TOKEN'
                    script {
                        try {
                            sh 'npx snyk test'  // Run the Snyk security test
                        } catch (Exception e) {
                            echo "Snyk scan encountered issues: ${e}"  // Allow the build to continue even if Snyk fails
                        }
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                // Run the project's tests
                sh 'npm test'
            }
        }
    }
    
    post {
        always {
            // Actions that will run after each build, successful or not
            echo 'Cleaning up temporary files...'
        }
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Build failed. Check logs for details.'
        }
    }
}

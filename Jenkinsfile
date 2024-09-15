pipeline {
    agent {
        docker {
            image 'node:16'
        }
    }
    stages {
        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run tests') {
            steps {
                sh 'npm test'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Security Scan') {
            steps {
                // Inject Snyk API token from Jenkins credentials
                withCredentials([string(credentialsId: 'snyk_api_token', variable: 'SNYK_TOKEN')]) {
                    // Run Snyk security test on package.json
                    sh 'npx snyk test --file=package.json --severity-threshold=high'
                }
            }
        }
    }
}


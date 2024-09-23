pipeline {
    agent {
        docker { image 'node:16' }
    }
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install --save'
            }
        }
        stage('Snyk Security Scan') {
            steps {
                withCredentials([string(credentialsId: 'snyk_api_token', variable: 'SNYK_TOKEN')]) {
                    sh 'npx snyk auth $SNYK_TOKEN'
                    sh 'npx snyk test'
                }
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
    }
}

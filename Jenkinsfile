pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Replace 'github-pat' with the ID of the Jenkins credential you created
                git branch: 'main', 
                    url: 'https://github.com/MilesOG22/8.2CDevSecOps.git', 
                    credentialsId: 'github-pat'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Continue even if tests fail
                sh 'npm test || true'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                // Generate coverage report, continue even if it fails
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                // Run security scan
                sh 'npm audit || true'
            }
        }
    }
}

pipeline {
  agent any

stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/MilesOG22/8.2CDevSecOps.git',
                    credentialsId: 'github-pat'
            }
        }

    stage('Install Dependencies') {
      steps {
        sh 'npm ci'            // use clean install (faster/consistent), falls back to npm install if package-lock mismatch
      }
    }

    stage('Run Tests') {
      steps {
        // save output to a file for submission; continue on failure
        sh 'npm test 2>&1 | tee test-output.log || true'
        archiveArtifacts artifacts: 'test-output.log', fingerprint: true
      }
    }

    stage('Generate Coverage Report') {
      steps {
        sh 'npm run coverage 2>&1 | tee coverage-output.log || true'
        archiveArtifacts artifacts: 'coverage-output.log', fingerprint: true
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        sh 'npm audit --audit-level=low 2>&1 | tee audit-output.log || true'
        archiveArtifacts artifacts: 'audit-output.log', fingerprint: true
      }
    }
  }

 post {
        success {
            emailext(
                subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """<p>Good news!</p>
                         <p>Build <b>${env.BUILD_NUMBER}</b> of job <b>${env.JOB_NAME}</b> succeeded.</p>
                         <p>Check console output: <a href="${env.BUILD_URL}console">${env.BUILD_URL}console</a></p>""",
                to: "milesco22@gmail.com"
            )
        }
        failure {
            emailext(
                subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """<p>Attention!</p>
                         <p>Build <b>${env.BUILD_NUMBER}</b> of job <b>${env.JOB_NAME}</b> failed.</p>
                         <p>Check console output: <a href="${env.BUILD_URL}console">${env.BUILD_URL}console</a></p>""",
                to: "milesco22@gmail.com"
            )
        }
  }
}

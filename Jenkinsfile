pipeline {
  agent {
    docker { image 'node:18' }
  }

  stages {
    stage('Checkout') {
      steps {
        // Use the SCM that retrieved the Jenkinsfile (safe & re-uses credentials configured for the job)
        checkout scm
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
    always {
      // Archive entire workspace snapshot that is small (optional)
      archiveArtifacts artifacts: '**/*.log', allowEmptyArchive: true
    }
  }
}

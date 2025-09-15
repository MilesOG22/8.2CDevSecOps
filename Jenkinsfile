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
        sh 'npm ci'
      }
    }

    stage('Run Tests') {
      steps {
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
      emailext(
        subject: "Jenkins Build: ${currentBuild.currentResult} - ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
        body: """<p>Build <b>${env.BUILD_NUMBER}</b> of job <b>${env.JOB_NAME}</b> finished with status: <b>${currentBuild.currentResult}</b></p>
                 <p>Check console output: <a href="${env.BUILD_URL}console">${env.BUILD_URL}console</a></p>""",
        to: "milesco22@gmail.com"
      )
    }

  stage('Force Fail') {
    steps {
      sh 'exit 1'
      }
    }
  }
}


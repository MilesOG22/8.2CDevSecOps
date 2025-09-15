pipeline {
  agent any
  stages {
    stage('Test Email') {
      steps {
        emailext(
          subject: "Test Email from Jenkins",
          body: "This is a test email.",
          to: "milesco22@gmail.com"
        )
      }
    }
  }
}



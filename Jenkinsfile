pipeline {
  agent any

  tools {
    maven 'maven_3.9'
  }

  environment {
    version = env.BRANCH_NAME.split('/')[1].toString()
  }

  stages {
    stage('Calculate & Set Version') {
      when {
        branch 'release/*'
      }
      steps {
        sh "mvn versions:set -DnewVersion=${version}"
      }
    }
    stage('Build & Tests') {
      steps {
        sh "mvn test"
        sh "mvn verify"
      }
    }
    stage('Push to GitHub') {
      steps {
        script {
            def changes = sh(script: "git diff --quiet", returnStdout: true).trim()
            if (sh script: "git diff --quiet", returnStatus: true) {
                withCredentials([gitUsernamePassword(jobCredentialId:'dmpodporin-github-creds', gitToolName:'Default')]) {
                sh "git add ."
                sh "git commit -m 'Version automatically update to ${version}'"
                sh "git push --set-upstream origin ${env.BRANCH_NAME}"
            }
          }
        }
      }
    }
  }
  post {
    success {
      slackSend channel: '#jenkins',
                color: '#36a64f',
                message: "Build Succeeded for ${env.BRANCH_NAME}"
    }
    failure {
      slackSend channel: '#jenkins',
                color: '#ff0000',
                message: "Build Failed for ${env.BRANCH_NAME}"
    }
  }
}
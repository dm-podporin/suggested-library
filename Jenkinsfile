pipeline {
    agent any

    tools {
        maven 'maven_3.9'
    }

    stages {
        stage('Calculate & Set Version') {
            when {
                branch 'release/*'
            }
            steps {
                script {
                    // def branchName = env.BRANCH_NAME
                    def version = env.BRANCH_NAME.split('/')[1].toString()
                    sh "mvn versions:set -DnewVersion=${version}"
                }
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
                    def changes = sh "git diff -q"
                    }
                step {
                    if (changes) {
                    withCredentials([gitUsernamePasword(jobCredentialId:'dmpodporin-github-creds)', gittToolName:'Deafult')]) {
                        sh "git add."
                        sh "git commit -m 'Version automaticaly update to${version}'"
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
                      message: "Build Succeeded for ${branchName}"
        }
        failure {
            slackSend channel: '#jenkins',
                      color: '#ff0000',
                      message: "Build Failed for ${branchName}"
        }
    }
    
}
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
                    def branchName = env.BRANCH_NAME
                    def version = branchName.split('/')[1].toString()
                    echo version
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
    }
    post {
        success {
            slackSend channel: '#jenkins',
                      color: '#36a64f',
                      message: "Build Succeeded for ${env.JOB_NAME} (${env.BUILD_NUMBER})"
        }
        failure {
            slackSend channel: '#jenkins',
                      color: '#ff0000',
                      message: "Build Failed for ${env.JOB_NAME} (${env.BUILD_NUMBER})"
        }
    }
    
}
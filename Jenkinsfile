pipeline {
    agent any

    stages {
        stage('Calculate & Set Version') {
            when {
                branch 'release/*'
            }
            steps {
                script {
                    def branchName = env.BRANCH_NAME
                    def version = branchName.split('/')[1]
                    def newVersion = "${major}.${minor}.${patch}"
                    sh "mvn versions: set -DnewVersion=${newVersion}"
                    sh "git commit -a -m 'Update version to ${newVersion}'"
                    sh "git push"
                }
            }
        }
        stage('Build & Test') {
            steps {
                sh "mvn clean install"
            }
        }
    }
    // post {
    //     success {
    //         slackSend channel: '#builds',
    //                   color: '#36a64f',
    //                   message: "Build Succeeded for ${env.JOB_NAME} (${env.BUILD_NUMBER})"
    //     }
    //     failure {
    //         slackSend channel: '#builds',
    //                   color: '#ff0000',
    //                   message: "Build Failed for ${env.JOB_NAME} (${env.BUILD_NUMBER})"
    //     }
    // }
    
}
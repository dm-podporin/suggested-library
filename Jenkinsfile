pipeline {
    agent any

    tools {
        maven 'Maven 3.6.2'
    }
    
    stages {
        stage('Calculate & Set Version') {
            when {
                branch 'release/*'
            }
            steps {
                script {
                    def branchName = env.BRANCH_NAME
                    def version = branchName.split('/')[1]
                    sh "mvn versions: set -DnewVersion=${version}"
                    sh "git commit -a -m 'Update version to ${version}'"
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
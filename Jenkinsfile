pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                echo 'Code cloned successfully! - v3'
            }
        }
        stage('Build') {
            steps {
                sh 'echo Build Done'
            }
        }
        stage('Test') {
            steps {
                sh 'echo Tests Passed'
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo Deployed!'
            }
        }
    }

    post {
    success {
        slackSend color: 'good', message: "✅ Build Success! - ${env.JOB_NAME} #${env.BUILD_NUMBER}"
    }
    failure {
        slackSend color: 'danger', message: "❌ Build Failed! - ${env.JOB_NAME} #${env.BUILD_NUMBER}"
        }
    }
}

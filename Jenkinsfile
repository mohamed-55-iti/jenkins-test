pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                echo 'Code cloned successfully!'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-app:latest .'
            }
        }
        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8090:80 --name my-app my-app:latest'
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

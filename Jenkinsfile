pipeline {
    agent { label 'agent-01' }

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
                sh 'docker stop my-app || true'
                sh 'docker rm my-app || true'
                sh 'docker run -d -p 8090:80 --name my-app my-app:latest'
            }
        }
    }

    post {
        success {
            slackSend color: 'good', message: "✅ Build Success on Agent-01! - ${env.JOB_NAME} #${env.BUILD_NUMBER}"
        }
        failure {
            slackSend color: 'danger', message: "❌ Build Failed! - ${env.JOB_NAME} #${env.BUILD_NUMBER}"
        }
    }
}

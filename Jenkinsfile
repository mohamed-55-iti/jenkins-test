 
pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                echo 'Code cloned successfully! - v2'
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
            echo 'All stages passed!'
        }
    }
}

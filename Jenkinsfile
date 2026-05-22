
pipeline {

    agent any

    environment {

        DOCKERHUB_USER = 'mohamed11755'

        IMAGE_NAME     = 'jenkins-test'

        GIT_REPO       = 'https://github.com/mohamed-55-iti/jenkins-test.git'

    }

    stages {

        stage('Clone') {

            steps {

                git branch: 'main', url: "${GIT_REPO}"

            }

        }

        stage('Build Image') {

            steps {

                sh "docker build -t ${DOCKERHUB_USER}/${IMAGE_NAME}:${BUILD_NUMBER} ."

                sh "docker tag ${DOCKERHUB_USER}/${IMAGE_NAME}:${BUILD_NUMBER} ${DOCKERHUB_USER}/${IMAGE_NAME}:latest"

            }

        }

        stage('Push to DockerHub') {

            steps {

                withCredentials([usernamePassword(

                    credentialsId: 'dockerhub-creds',

                    usernameVariable: 'USER',

                    passwordVariable: 'PASS'

                )]) {

                    sh 'echo $PASS | docker login -u $USER --password-stdin'

                    sh "docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:${BUILD_NUMBER}"

                    sh "docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:latest"

                }

            }

        }

        stage('Update Manifest') {

            steps {

                withCredentials([usernamePassword(

                    credentialsId: 'github-creds',

                    usernameVariable: 'GIT_USER',

                    passwordVariable: 'GIT_PASS'

                )]) {

                    sh """

                        git fetch origin

                        git checkout gitops

                        git pull origin gitops

                        sed -i 's|image:.*|image: ${DOCKERHUB_USER}/${IMAGE_NAME}:${BUILD_NUMBER}|' deployment.yaml

                        git config user.email "jenkins@ci.com"

                        git config user.name "Jenkins"

                        git add deployment.yaml

                        git commit -m "Update image to build ${BUILD_NUMBER}"

                        git push https://\${GIT_USER}:\${GIT_PASS}@github.com/mohamed-55-iti/jenkins-test.git gitops

                    """

                }

            }

        }

    }

    post {

        success {

            mail to: 'YOUR_EMAIL@gmail.com',

                 subject: "SUCCESS - Build #${BUILD_NUMBER}",

                 body: "Build ${BUILD_NUMBER} succeeded!\n\nJob: ${JOB_NAME}\nURL: ${BUILD_URL}"

        }

        failure {

            mail to: 'mmmnnn11755@gmail.com',

                 subject: "FAILED - Build #${BUILD_NUMBER}",

                 body: "Build ${BUILD_NUMBER} failed!\n\nJob: ${JOB_NAME}\nConsole: ${BUILD_URL}console"

        }

    }

}


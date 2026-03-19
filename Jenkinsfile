pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "akashak0717/real-estate-app"
        DOCKER_TAG   = "${BUILD_NUMBER}"
        EC2_USER     = "ec2-user"
    }

    triggers {
        githubPush()
    }

    stages {

        stage('Clone Repository') {
            steps {
                echo 'Cloning repository from GitHub...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                sh "docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} ${DOCKER_IMAGE}:latest"
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing image to Docker Hub...'
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                    sh "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }

        stage('Deploy to AWS EC2') {
            steps {
                echo 'Deploying to AWS EC2...'
                withCredentials([
                    string(credentialsId: 'EC2_PUBLIC_IP', variable: 'EC2_HOST'),
                    sshUserPrivateKey(credentialsId: 'ec2-ssh-key', keyFileVariable: 'SSH_KEY')
                ]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no -i $SSH_KEY ${EC2_USER}@${EC2_HOST} '
                            docker pull ${DOCKER_IMAGE}:latest &&
                            docker stop real-estate || true &&
                            docker rm real-estate || true &&
                            docker run -d --name real-estate -p 80:80 --restart always ${DOCKER_IMAGE}:latest
                        '
                    """
                }
            }
        }

        stage('Cleanup') {
            steps {
                echo 'Cleaning up local Docker images...'
                sh "docker rmi ${DOCKER_IMAGE}:${DOCKER_TAG} || true"
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful! App is live on AWS EC2.'
        }
        failure {
            echo '❌ Pipeline failed. Check logs above.'
        }
        always {
            cleanWs()
        }
    }
}
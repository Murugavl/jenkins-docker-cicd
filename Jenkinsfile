pipeline {

    agent any

    environment {
        IMAGE_NAME = 'jenkins-flask-demo'
        CONTAINER_NAME = 'jenkins-flask-demo'
        HOST_PORT = '5000'
        CONTAINER_PORT = '5000'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Docker Version') {
            steps {
                sh 'docker --version'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'

                sh '''
                    docker build \
                    -t ${IMAGE_NAME}:${BUILD_NUMBER} \
                    .
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'

                sh '''
                    docker rm -f ${CONTAINER_NAME} || true
                '''

                sh '''
                    docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p ${HOST_PORT}:${CONTAINER_PORT} \
                    ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Health Check') {
            steps {
                echo 'Checking application health...'

                sh '''
                    sleep 5
                    curl -f http://localhost:${HOST_PORT}/health
                '''
            }
        }

    }

    post {

        success {
            echo '================================'
            echo 'DEPLOYMENT SUCCESSFUL'
            echo '================================'

            sh 'docker ps'
        }

        failure {
            echo '================================'
            echo 'PIPELINE FAILED'
            echo '================================'

            sh 'docker ps -a || true'
            sh 'docker logs ${CONTAINER_NAME} || true'
        }
    }
}

pipeline {

    agent any

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
                    -t jenkins-flask-demo:${BUILD_NUMBER} \
                    .
                '''
            }
        }

        stage('Docker Images') {
            steps {
                sh 'docker images'
            }
        }

    }
}

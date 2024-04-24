pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-build-push')
        DOCKER_TLS_VERIFY = '0'
    }
    stages {
        stage('Unset DOCKER_TLS_VERIFY') {
            steps {
                sh 'unset DOCKER_TLS_VERIFY'
            }
        }
        stage('Build') {
            steps {
                script {
                    docker.build('docker-agent-python')
                }
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t tiagodfigueiredo/jenkins761:latest .'
            }
        }
        stage ('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }    
        stage('Push') {
            steps {
                sh 'docker push tiagodfigueiredo/jenkins761:latest'
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}

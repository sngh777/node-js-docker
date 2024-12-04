pipeline {
    agent any 
    environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerID')
    }
    stages { 
        stage('SCM Checkout') {
            steps{
             git branch: 'main', url: 'https://github.com/sngh777/node-js-docker.git'
            }
        }

        stage('Build docker image') {
            steps {  
                sh 'docker build -t nodeapp:$BUILD_NUMBER .'
            }
        }
        stage('login to dockerhub') {
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image') {
            steps{
                sh 'docker push nsinghvs08/test:nodeapp'
            }
        }
}
post {
        always {
            sh 'docker logout'
        }
    }
}


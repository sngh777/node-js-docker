pipeline {
    agent any 
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerID')
    }
    stages { 
        stage('SCM Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sngh777/node-js-docker.git'
            }
        }

        stage('Build docker image') {
            steps {  
                sh 'docker build -t nsinghvs08/nodeapp:$BUILD_NUMBER .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push image') {
            steps {
                sh 'docker push nsinghvs08/nodeapp:$BUILD_NUMBER'
            }
        }

        stage('Run Docker container locally') {
            steps {
                sh 'docker run -d -p 3000:3000 --name nodeapp-$BUILD_NUMBER nsinghvs08/nodeapp:$BUILD_NUMBER'
            }
        }
    }

    post {
        always {
            // Clean up: Logout from Docker Hub
            sh 'docker logout'

            // Stop and remove the container after the job completes
            sh 'docker stop nodeapp-$BUILD_NUMBER || true'
            sh 'docker rm nodeapp-$BUILD_NUMBER || true'
        }
    }
}

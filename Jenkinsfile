pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "dharssan/devops-app"
    }

    stages {

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Run Container Test') {
            steps {
                //sh 'docker run -d -p 3000:3000 $DOCKER_IMAGE'
                sh 'docker rm $(docker ps -aq) || true'
                sh 'docker run -d -p 3000:3000 $DOCKER_IMAGE'
                sh 'sleep 5'
                sh 'docker ps'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USERNAME',
                    passwordVariable: 'PASSWORD'
                )]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
    }
}

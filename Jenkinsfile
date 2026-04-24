pipeline {
    agent any

    environment {
        IMAGE_NAME = "shantha19/devops-webapp"
        CONTAINER_NAME = "webcontainer"
    }

    stages {
        stage('Pull Source Code') {
            steps {
                git branch: 'main', url: 'https://github.com/MShantha-19/devops-webapp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-webapp .'
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh 'docker tag devops-webapp $IMAGE_NAME:latest'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker rm -f $CONTAINER_NAME || true
                docker run -d -p 80:80 --name $CONTAINER_NAME $IMAGE_NAME:latest
                '''
            }
        }
    }
}

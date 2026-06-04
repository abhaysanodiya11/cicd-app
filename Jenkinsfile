pipeline {
    agent any

    enviroment {
        DOCKER_IMAGE = "abhaysanodiya11/cicd-app"
    }

    stages {

        stage('Clone Code') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    sh "docker push $DOCKER_IMAGE"
                }
            }
        }

        stage(' Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8/s deployment.yml'
            }
        }
    }
}
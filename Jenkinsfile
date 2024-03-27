pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = 'todo-app'
        DOCKER_HUB_REPO = 'leonswww/todo-app'
    }
    stages {
        stage('Clone the repo') {
            steps {
                echo 'Cloning the repository:'
                git 'https://github.com/saktil/cicd-jenkins.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building the ToDo application on Docker'
                sh 'docker build . -t $DOCKER_IMAGE_NAME'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the application on Docker'
                sh 'docker run -p 8000:8000 -d $DOCKER_IMAGE_NAME'
            }
        }
        stage('Upload image') {
            steps {
                echo 'Uploading Docker image to Docker Hub'
                withCredentials([usernamePassword(credentialsId: 'dockerHub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                    sh "docker tag $DOCKER_IMAGE_NAME $DOCKER_HUB_REPO:latest"
                    sh "docker push $DOCKER_HUB_REPO:latest"
                }
            }
        }
        stage('Remove old image') {
            steps {
                echo 'Removing old Docker image'
                sh "docker rmi $DOCKER_HUB_REPO:latest"
            }
        }
    }
}

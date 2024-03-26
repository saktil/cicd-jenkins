pipeline {
    agent any
    stages {
        stage('Clone the repo') {
            steps {
                echo 'Cloning the repository:'
                git 'https://github.com/mudit097/node-todo-cicd.git'
            }
        }

        stage('SCM') {
            checkout scm
        }
        stage('SonarQube Analysis') {
            def scannerHome = tool 'SonarScanner';
            withSonarQubeEnv() {
            sh "${scannerHome}/bin/sonar-scanner"
            }
        }

        stage('Build') {
            steps {
                echo 'Building the ToDo application on Docker'
                sh 'docker build . -t todo-app'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the application on Docker'
                sh 'docker run -p 8000:8000 -d todo-app'
            }
        }
        stage('Upload image') {
            steps {
                echo 'Uploading Docker image to Docker Hub'
                withCredentials([usernamePassword(credentialsId: 'dockerHub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                    sh "docker tag todo-app leonswww/todo-app:latest"
                    sh "docker push leonswww/todo-app:latest"
                }
            }
        }
    }
}
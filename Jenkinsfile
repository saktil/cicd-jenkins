pipeline {
    agent any
    stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/LondheShubham153/node-todo-cicd.git", branch: "master"
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
        stage("Build and Test"){
            steps{
                sh "docker build . -t node-app-test-new"
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag node-app-test-new ${env.dockerHubUser}/node-app-test-new:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}

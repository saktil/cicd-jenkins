pipeline {
    agent any
    environment {
        scannerHome = tool 'SonarScanner'
        dockerImage = 'node-app-test-new'
        dockerHubCreds = credentials('dockerHub')
    }
    stages {
        stage("Clone Code") {
            steps {
                git url: "https://github.com/saktil/cicd-jenkins.git", branch: "master"
            }
        }
        stage("SonarQube Analysis") {
            steps {
                withSonarQubeEnv() {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        stage("Build and Test") {
            steps {
                script {
                    sh "docker build . -t ${dockerImage}"
                }
            }
        }
        stage("Push to Docker Hub") {
            steps {
                script {
                    docker.withRegistry('', dockerHubCreds) {
                        sh "docker tag ${dockerImage} ${dockerHubCreds.username}/node-app-test-new:latest"
                        sh "docker push ${dockerHubCreds.username}/node-app-test-new:latest"
                    }
                }
            }
        }
        stage("Deploy") {
            steps {
                script {
                    sh "docker-compose down"
                    sh "docker-compose up -d"
                }
            }
        }
    }
}

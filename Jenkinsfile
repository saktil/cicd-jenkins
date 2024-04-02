pipeline {
    agent any
    tools {
        maven 'Maven'
    }

    environment {
        SONARQUBE_SERVER_URL = 'http://8.215.42.245:9000'
    }

    stages {
        stage('Clone the repo') {
            steps {
                echo 'Cloning the repository:'
                git 'https://github.com/saktil/cicd-jenkins.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    try {
                        withSonarQubeEnv('sonarqube-server') {
                            sh '''mvn clean verify sonar:sonar \
                                   -Dsonar.projectKey=testing \
                                   -Dsonar.projectName='testing' \
                                   -Dsonar.host.url=${SONARQUBE_SERVER_URL}'''
                            echo 'SonarQube Analysis Completed'
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        echo "SonarQube analysis failed: ${e.message}"
                    }
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building the ToDo application on Docker'
                sh 'docker build . -t todo-app'
            }
        }
    }
}
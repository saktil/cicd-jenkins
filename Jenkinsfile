pipeline {
    agent any
    environment {
        SONARQUBE_SERVER_URL = 'http://172.28.164.35:9000'
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

        stage('Deploy') {
            steps {
                echo 'Deploying the application on Docker'
                sh 'docker run -p 8000:8000 -d todo-app'
            }
        }
    }
}

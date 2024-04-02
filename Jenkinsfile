pipeline {
    agent any
    tools {
        maven 'Maven'
    }

    environment {
        SONARQUBE_SERVER_URL = 'http://8.215.42.245:9000'
    }

    stages {
        stage('Git Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/saktil/maven-sonar.git']])
                echo 'Git Checkout Completed'
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
    }
}

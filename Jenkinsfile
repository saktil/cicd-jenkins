pipeline {
    agent any
    stages {
        stage('Clone the repo') {
            steps {
                echo 'Cloning the repository:'
                git 'https://github.com/saktil/cicd-jenkins.git'
            }
        }

        stage('SonarQube analysis') {
            steps {
                script {
                    // Downloading and configuring SonarScanner
                    sh 'curl -L -o sonar-scanner-cli.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.6.2.2472-linux.zip'
                    sh 'unzip sonar-scanner-cli.zip'
                    sh 'mv sonar-scanner-4.6.2.2472-linux sonar-scanner'

                    // Running SonarScanner
                    withSonarQubeEnv('My SonarQube Server') {
                        sh './sonar-scanner/bin/sonar-scanner'
                    }
                }
            }
        }

        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}

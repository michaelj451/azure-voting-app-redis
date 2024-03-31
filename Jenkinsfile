pipeline {
    agent any

    stages {
        stage('Verify Branch') {
            steps {
                echo "$GIT_BRANCH"
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    // Change to the azure-vote directory and execute Docker commands
                    dir('azure-vote') {
                        sh 'docker images -a'
                        sh 'docker build -t jenkins-pipeline .'
                        sh 'docker images -a'
                    }
                }
            }
        }
    }
}
pipeline {
    agent any

    stages {
        stage('Verify Branch') {
            steps {
                echo "$GIT_BRANCH"
            }
        }
        stage('Docker Build') {
            agent {
                docker {
                    image 'docker:latest'
                }
            }
            steps {
                sh(script: 'docker images -a')
            }
        }
    }
}
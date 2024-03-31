pipeline {
    agent any

    stages {
        stage('Verify Branch') {
            steps {
                echo "$GIT_BRANCH"
            }
        }
        stage('Docker Test') {
            steps {
                sh(script: 'docker images -a')
            }
        }
        stage('Docker image cleanup if name is already in use') {
            steps {
                sh(script: """
                    if docker images -q jenkins-pipeline; then
                        echo "Image already exists, removing it"
                        docker rmi jenkins-pipeline
                        docker images -a
                    else
                        echo "Image does not exist"
                    fi
                """)
            }
        }
        stage('Docker Build') {
            steps {
                sh(script: """
                    cd azure-vote/
                    docker images -a
                    docker build -t jenkins-pipeline .
                    docker images -a
                    cd ..
                """)
            }
        }
    }
}
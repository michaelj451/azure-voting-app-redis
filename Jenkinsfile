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
                    if docker images -q jenkins-pipeline | grep -q .; then
                        echo "Image already exists, removing it"
                        docker rmi jenkins-pipeline || true
                    else
                        echo "Image does not exist, no action required"
                    fi
                    docker images -a
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
        stage('Start Test App') {
            steps {
                sh(script: """
                    # Start the app
                    ./scripts/test_container.sh
                """)
            }
            post {
                success {
                    echo 'Test app started successfully'
                }
                failure {
                    echo 'Failed to start test app'
                }
            }
        }
        stage('Run Tests') {
            steps {
                sh(script: """
                    # Run tests
                    pytest ./tests/test_sample.py
                """)
            }
        }
        stage('Stop Test App') {
            steps {
                sh(script: """
                    # Stop the app
                    ./scripts/stop_container.sh
                """)
            }
            post {
                success {
                    echo 'Test app stopped successfully'
                }
                failure {
                    echo 'Failed to stop test app'
                }
            }
        }
    }
}
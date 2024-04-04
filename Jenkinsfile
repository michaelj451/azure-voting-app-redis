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
        stage('Check Test App') {
            steps {
                sh(script: """
                    # Start the app
                    echo "Starting test app"
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
        stage('Start Test App') {
            steps {
                sh(script: """
                    # Run tests
                    docker-compose up -d
                    docker container ls
                """)
            }
            post {
                success {
                    echo 'Started test app successfully'
                }
                failure {
                    echo 'Failed to start test app'
                }
            }
        }
        stage('Stop Test App') {
            steps {
                sh(script: """
                    # Stop the app
                    docker-compose stop
                    docker container ls
                """)
            }
            post {
                success {
                    echo 'Stopped test app successfully'
                }
                failure {
                    echo 'Failed to stop test app'
                }
            }
        }
        // stage('Docker Prune') {
        //     steps {
        //         sh(script: 'docker system prune -a -f')
        //     }
        // }
        stage('Docker Push') {
            environment {
                // Credentials are for gitlab job credentials named gitlab_credentials
                DOCKER_REGISTRY = 'gitlab.mikeferguson.us:5050'
                DOCKER_IMAGE = 'jenkins-pipeline'
                DOCKER_TAG = "${env.BUILD_NUMBER}"
                DOCKER_CREDENTIALS_ID = credentials('docker-credentials')
            }

            steps {
                sh(script: """
                    echo "Mike Was Here"
                    echo "$DOCKER_CREDENTIALS_USR"
                    echo "$DOCKER_CREDENTIALS_PSW"
                """)
            }
        }
    }
}
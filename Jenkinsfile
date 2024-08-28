pipeline {
    agent {
        dockerfile {
            filename 'jenkins/Dockerfile'
            dir '.'
            additionalBuildArgs '--no-cache'
        }
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm
                }
            }
        }

        stage('Cleanup Previous Containers') {
            steps {
                script {
                    sh 'docker-compose down'
                }
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    sh 'docker-compose build'
                    sh 'docker-compose run web python manage.py test'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        always {
            script {
                // Clean up Docker resources
                echo 'Skipping Docker cleanup for now'
            }
        }
    }
}

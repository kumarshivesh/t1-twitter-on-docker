pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                script {
                    sh 'rm -rf *'
                    sh 'chown -R jenkins:jenkins /var/jenkins_home/workspace/mantletwitter'
                }
            }
        }

        stage('Checkout') {
            steps {
                script {
                    checkout scm
                    sh 'git status' // Verify that the repository was checked out
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
                sh 'docker system prune -a --volumes'
            }
        }
    }
}

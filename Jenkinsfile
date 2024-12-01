pipeline {
    agent any

    environment {
        CI = 'true'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Running Build Stage...'
                bat 'npm install'
            }
        }
        stage('Test') {
            steps {
                echo 'Running Test Stage...'
                bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/test.sh'
            }
        }
        stage('Deliver for development') {
            when {
                branch 'development'
            }
            steps {
                echo 'Delivering for Development...'
                bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/deliver-for-development.sh'
            }
            post {
                always {
                    input message: 'Finished using the web site? (Click "Proceed" to continue)'
                    timeout(time: 5, unit: 'MINUTES') {
                        echo 'Cleaning up development environment...'
                        bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/kill.sh'
                    }
                }
            }
        }
        stage('Deploy for production') {
            when {
                branch 'production'
            }
            steps {
                echo 'Deploying for Production...'
                bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/deploy-for-production.sh'
            }
            post {
                always {
                    input message: 'Finished using the web site? (Click "Proceed" to continue)'
                    timeout(time: 5, unit: 'MINUTES') {
                        echo 'Cleaning up production environment...'
                        bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/kill.sh'
                    }
                }
            }
        }
    }
}

pipeline {
    agent any

    environment {
        CI = 'true'
    }

    stages {
        stage('Build') {
            steps {
                bat 'npm install'
            }
        }
        stage('Test') {
            steps {
                bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/test.sh'
            }
        }

        stage('Deliver for development') {
            when {
                branch 'development'
            }
            steps {
                bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/deliver-for-development.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)' 
                timeout(time: 5, unit: 'MINUTES') // Correct timeout usage
                bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/kill.sh'
            }
        }

        stage('Deploy for production') {
            when {
                branch 'production'
            }
            steps {
                bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/deploy-for-production.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                timeout(time: 5, unit: 'MINUTES') // Correct timeout usage
                bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/kill.sh'
            }
        }
    }
}
pipeline {
    agent any

    environment {
        CI = 'true'
    }

    stages {
        stage('Build') {
            steps {
                echo "Running npm install..."
                bat 'npm install'
            }
        }

        stage('Test') {
            steps {
                echo "Executing test script..."
                bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/test.sh'
            }
        }

        stage('Deliver for development') {
            when {
                branch 'development'
            }
            steps {
                echo "Delivering for development..."
                bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/deliver-for-development.sh'

                echo "Waiting for user confirmation..."
                input message: 'Finished using the website? (Click "Proceed" to continue)'

                timeout(time: 5, unit: 'MINUTES') {
                    echo "Stopping the development server..."
                    bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/kill.sh'
                }
            }
        }

        stage('Deploy for production') {
            when {
                branch 'production'
            }
            steps {
                echo "Deploying for production..."
                bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/deploy-for-production.sh'

                echo "Waiting for user confirmation..."
                input message: 'Finished using the website? (Click "Proceed" to continue)'

                timeout(time: 5, unit: 'MINUTES') {
                    echo "Stopping the production server..."
                    bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/kill.sh'
                }
            }
        }
    }
}

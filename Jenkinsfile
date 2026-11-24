
pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out Laravel project from GitHub'

                git branch: 'main',
                    url: 'https://github.com/kannantncbe/laravel-chat-demo.git'
            }
        }

        stage('Install PHP Dependencies') {
            steps {
                echo 'Installing Composer dependencies'

                bat '''
                    call C:\\composer\\composer.bat install --no-interaction --prefer-dist --no-progress
                '''
            }
        }

        stage('Laravel Quality Checks and Tests') {
            steps {
                echo 'Running Pint, PHPStan and Laravel tests'

                bat '''
                    call C:\\composer\\composer.bat test
                '''
            }
        }

        stage('Install Frontend Dependencies') {
            steps {
                echo 'Installing Node dependencies'

                bat '''
                    npm ci
                '''
            }
        }

        stage('Frontend Production Build') {
            steps {
                echo 'Building frontend assets'

                bat '''
                    npm run build
                '''
            }
        }

        stage('Archive Build Artifacts') {
            steps {
                archiveArtifacts artifacts: 'public/build/**',
                    fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Laravel CI Pipeline completed successfully!'
        }

        failure {
            echo 'Laravel CI Pipeline failed. Check the stage console output.'
        }

        always {
            echo 'CI Pipeline execution finished.'
        }
    }
}

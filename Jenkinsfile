
pipeline {
    agent any

    environment {
        APP_ENV = 'testing'
        APP_KEY = 'base64:MTIzNDU2Nzg5MDEyMzQ1Njc4OTAxMjM0NTY3ODkwMTI='
    }

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    stages {
        stage('Install PHP Dependencies') {
            steps {
                echo 'Installing Composer dependencies'

                bat '''
                    call C:\\composer\\composer.bat install --no-interaction --prefer-dist --no-progress
                '''
            }
        }

        stage('Install Frontend Dependencies') {
            steps {
                echo 'Installing Node dependencies'

                bat 'npm ci'
            }
        }

        stage('Frontend Production Build') {
            steps {
                echo 'Building frontend assets'

                bat 'npm run build'
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


pipeline {
    agent any

    environment {
        APP_ENV = 'testing'
        APP_KEY = 'base64:MTIzNDU2Nzg5MDEyMzQ1Njc4OTAxMjM0NTY3ODkwMTI='
    }

    parameters {
        booleanParam(
            name: 'RUN_COMPOSER_INSTALL',
            defaultValue: true,
            description: 'Run Composer install'
        )

        booleanParam(
            name: 'RUN_NPM_INSTALL',
            defaultValue: true,
            description: 'Install Node dependencies using npm ci'
        )

        booleanParam(
            name: 'RUN_NPM_BUILD',
            defaultValue: true,
            description: 'Build frontend production assets'
        )

        booleanParam(
            name: 'RUN_CONFIG_CLEAR',
            defaultValue: false,
            description: 'Run php artisan config:clear'
        )

        booleanParam(
            name: 'RUN_CONFIG_CACHE',
            defaultValue: false,
            description: 'Run php artisan config:cache'
        )

        booleanParam(
            name: 'RUN_CACHE_CLEAR',
            defaultValue: false,
            description: 'Run php artisan cache:clear'
        )

        booleanParam(
            name: 'RUN_OPTIMIZE_CLEAR',
            defaultValue: false,
            description: 'Run php artisan optimize:clear'
        )
    }

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    stages {

        stage('Composer Install') {
            when {
                expression { params.RUN_COMPOSER_INSTALL }
            }
            steps {
                echo 'Installing Composer dependencies'

                bat '''
                    call C:\\composer\\composer.bat install --no-interaction --prefer-dist --no-progress
                '''
            }
        }

        stage('NPM Install') {
            when {
                expression { params.RUN_NPM_INSTALL }
            }
            steps {
                echo 'Installing Node dependencies'

                bat 'npm ci'
            }
        }

        stage('NPM Build') {
            when {
                expression { params.RUN_NPM_BUILD }
            }
            steps {
                echo 'Building frontend assets'

                bat 'npm run build'
            }
        }

        stage('Laravel Optimize Clear') {
            when {
                expression { params.RUN_OPTIMIZE_CLEAR }
            }
            steps {
                bat 'php artisan optimize:clear'
            }
        }

        stage('Laravel Config Clear') {
            when {
                expression { params.RUN_CONFIG_CLEAR }
            }
            steps {
                bat 'php artisan config:clear'
            }
        }

        stage('Laravel Cache Clear') {
            when {
                expression { params.RUN_CACHE_CLEAR }
            }
            steps {
                bat 'php artisan cache:clear'
            }
        }

        stage('Laravel Config Cache') {
            when {
                expression { params.RUN_CONFIG_CACHE }
            }
            steps {
                bat 'php artisan config:cache'
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

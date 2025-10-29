







pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:/opt/homebrew/bin:$PATH"
        COLLECTION = 'MyCollection.json'
        ENV_FILE   = 'environment.json'
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Pulling latest code from GitHub...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing Newman (if not already installed)...'
                sh '''
                    if ! command -v newman &> /dev/null
                    then
                      echo "Newman not found, installing globally..."
                      npm install -g newman
                    else
                      echo "Newman is already installed."
                    fi
                '''
            }
        }

        stage('Continuous Integration - Run Postman Tests') {
            steps {
                echo 'Running Postman collection tests...'
                sh '''
                    echo ">>> Starting Newman Test Run"
                    newman run $COLLECTION --environment $ENV_FILE || exit 1
                '''
            }
        }

        stage('Continuous Deployment - Dummy Deploy') {
            when {
                expression { currentBuild.currentResult == 'SUCCESS' }
            }
            steps {
                echo 'All tests passed. Deploying application...'
                sh '''
                    echo ">>> Deploy simulation complete!"
                    echo "This confirms CI/CD pipeline automation is working!"
                '''
            }
        }
    }

    post {
        success {
            echo '🎉 Build and deployment successful!'
        }
        failure {
            echo '❌ Build failed. Check logs above.'
        }
    }
}

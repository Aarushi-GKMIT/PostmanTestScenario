pipeline {
    agent any

    environment {
        // Add Node and Newman paths if needed
        PATH = "/usr/local/bin:/opt/homebrew/bin:$PATH"
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📦 Cloning repository...'
                checkout scm
            }
        }

        stage('Install Newman') {
            steps {
                echo '📦 Installing Newman...'
                sh '''
                    if ! command -v newman &> /dev/null
                    then
                        npm install -g newman
                    else
                        echo "✅ Newman already installed"
                    fi
                '''
            }
        }

        stage('Run API Tests (CI)') {
            steps {
                echo '🚀 Running Postman tests...'
                sh '''
                    newman run MyCollection.json \
                        --environment environment.json \
                        --reporters cli,junit \
                        --reporter-junit-export results.xml
                '''
            }
        }

        stage('Deploy (CD)') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                echo '🚢 All tests passed! Starting deployment...'
                sh '''
                    echo "✅ Deployment simulated — put your deploy commands here."
                    # Example: scp files to server, or docker build/push commands
                '''
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed! Check logs for details.'
        }
    }
}





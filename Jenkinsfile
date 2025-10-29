// 🔁 Test Automation Commit #3 — New CI/CD flow

pipeline {
    agent any

    environment {
        COLLECTION = 'MyCollection.json'
        ENV_FILE   = 'environment.json'
        REPORT     = 'newman-report.html'
    }

    stages {

        stage('Checkout from GitHub') {
            steps {
                echo "📦 Fetching latest code from GitHub..."
                checkout scm
            }
        }

        stage('Setup Environment') {
            steps {
                echo "⚙️ Checking Newman installation..."
                sh '''
                    if ! command -v newman &> /dev/null
                    then
                        echo "Installing Newman globally..."
                        npm install -g newman newman-reporter-html
                    else
                        echo "✅ Newman already installed"
                    fi
                '''
            }
        }

        stage('Postman Regression Tests') {
            steps {
                echo "🧪 Running Postman tests at $(date)"
                sh '''
                    echo "Starting Newman collection run..."
                    newman run $COLLECTION --environment $ENV_FILE \
                        --reporters cli,html --reporter-html-export $REPORT
                '''
            }
        }

        stage('Continuous Deployment (Simulated)') {
            when {
                expression { currentBuild.currentResult == 'SUCCESS' }
            }
            steps {
                echo "🚀 Deploying build to staging (simulation)..."
                sh '''
                    echo "Deployment timestamp: $(date)"
                    echo ">>> Deployment complete!"
                '''
            }
        }
    }

    post {
        success {
            echo "🎉 CI/CD pipeline executed successfully!"
            echo "HTML report generated: $REPORT"
        }
        failure {
            echo "❌ Build failed. Please check console output or Newman logs."
        }
    }
}







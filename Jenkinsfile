pipeline {
    agent any

    environment {
        PATH = "$PATH:/usr/local/bin:/opt/homebrew/bin"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Aarushi-GKMIT/PostmanTestScenario.git'
            }
        }

        stage('Run API Tests') {
            steps {
                sh '''
                    echo "🚀 Running Postman collection..."
                    newman run MyCollection.json --environment environment.json \
                    --reporters cli,html \
                    --reporter-html-export newman-report.html
                '''
            }
        }

        stage('Archive Results') {
            steps {
                archiveArtifacts artifacts: 'newman-report.html', fingerprint: true
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished (success or fail).'
        }
        success {
            echo '✅ All tests passed successfully!'
        }
        failure {
            echo '❌ Tests failed. Please check newman-report.html for details.'
        }
    }
}


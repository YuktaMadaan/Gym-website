pipeline {
    agent any

    environment {
        AZ_USER = credentials('REDACTED')     // Jenkins credential ID for FTPS username
        AZ_PASS = credentials('REDACTED')     // Jenkins credential ID for FTPS password
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "📦 Cloning Gym Website from GitHub..."
                git branch: 'main', url: 'https://github.com/YuktaMadaan/Gym-website.git'
            }
        }

        stage('Deploy to Azure') {
            steps {
                echo "🚀 Uploading index.html to Azure Web App..."
                script {
                    bat """
                    curl -T index.html "ftps://waws-prod-hk1-081.ftp.azurewebsites.windows.net/site/wwwroot/" --user "%AZ_USER%:%AZ_PASS%"
                    """
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful! Visit your website to verify.'
        }
        failure {
            echo '❌ Deployment failed. Check Jenkins or Azure logs for details.'
        }
    }
}

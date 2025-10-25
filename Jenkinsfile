pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo "📦 Checking out source code..."
                git branch: 'main', url: 'https://github.com/YuktaMadaan/Gym-website.git'
            }
        }

        stage('Deploy to Azure via FTP') {
            steps {
                echo "🚀 Deploying to Azure App Service..."
                withCredentials([usernamePassword(credentialsId: 'azure-ftp-creds', usernameVariable: 'FTP_USER', passwordVariable: 'FTP_PASS')]) {
                    bat """
                        curl -T index.html -u %FTP_USER%:%FTP_PASS% ftp://waws-prod-hk1-081.ftp.azurewebsites.windows.net/site/wwwroot/index.html --ssl
                    """
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Deployment failed. Check Jenkins or Azure logs for details.'
        }
    }
}

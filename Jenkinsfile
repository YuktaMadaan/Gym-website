pipeline {
    agent any

    environment {
        FTP_HOST = 'waws-prod-hk1-081.ftp.azurewebsites.windows.net'
        FTP_DIR  = '/site/wwwroot'
        CREDENTIALS_ID = 'azure-ftp-creds'
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo '📦 Checking out source code...'
                git branch: 'main', url: 'https://github.com/YuktaMadaan/Gym-website.git'
            }
        }

        stage('Deploy to Azure via FTP') {
            steps {
                echo '🚀 Deploying to Azure App Service...'
                withCredentials([usernamePassword(credentialsId: "${CREDENTIALS_ID}", usernameVariable: 'FTP_USER', passwordVariable: 'FTP_PASS')]) {
                    // Upload all website files using curl
                    bat """
                        curl -T index.html -u %FTP_USER%:%FTP_PASS% ftp://${FTP_HOST}${FTP_DIR}/index.html
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

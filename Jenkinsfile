pipeline {
    agent any

    environment {
        FTP_SITE = 'waws-prod-hk1-081.ftp.azurewebsites.windows.net'
        FTP_PATH = '/site/wwwroot/'
    }

    stages {
        stage('Checkout') {
            steps {
                echo '🔍 Pulling code from GitHub...'
                git branch: 'main', url: 'https://github.com/YuktaMadaan/Gym-website.git'
            }
        }

        stage('Deploy to Azure') {
            steps {
                echo '🚀 Uploading index.html to Azure Web App via FTP...'
                withCredentials([usernamePassword(credentialsId: 'azure-ftp-creds', usernameVariable: 'FTP_USER', passwordVariable: 'FTP_PASS')]) {
                    script {
                        if (isUnix()) {
                            sh "curl -T index.html -u $FTP_USER:$FTP_PASS ftp://$FTP_SITE$FTP_PATH"
                        } else {
                            bat "curl -T index.html -u %FTP_USER%:%FTP_PASS% ftp://%FTP_SITE%%FTP_PATH%"
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful! Visit your site on Azure.'
        }
        failure {
            echo '❌ Deployment failed. Check Jenkins or Azure logs.'
        }
    }
}

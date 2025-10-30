pipeline {
    agent any

    environment {
        // CHANGED: Using the standard Azure Web App FTP hostname format
        // This is a common workaround for DNS instability with the longer regional address.
        FTP_SITE = 'ftp.gymweb.azurewebsites.net' // <<< VERIFY THIS HOSTNAME MATCHES YOUR APP NAME
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
                        def ftpAuth = "${FTP_USER}:${FTP_PASS}" 
                        
                        if (isUnix()) {
                            sh "curl -v -T index.html -u ${ftpAuth} ftp://${FTP_SITE}${FTP_PATH}"
                        } else {
                            // Correct Windows syntax to prevent login errors (no surrounding quotes)
                            bat "curl -v -T index.html -u ${ftpAuth} ftp://${FTP_SITE}${FTP_PATH}" 
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
            echo '❌ Deployment failed. Check Jenkins or Azure logs for specific errors.'
        }
    }
}

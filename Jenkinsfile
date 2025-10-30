pipeline {
    agent any

    environment {
        // Define FTP connection details as environment variables
        FTP_SITE = 'waws-prod-hk1-081.ftp.azurewebsites.windows.net'
        FTP_PATH = '/site/wwwroot/'
    }

    stages {
        stage('Checkout') {
            steps {
                echo '🔍 Pulling code from GitHub...'
                // Using the declarative git step for initial SCM checkout is best practice
                git branch: 'main', url: 'https://github.com/YuktaMadaan/Gym-website.git'
            }
        }

        stage('Deploy to Azure') {
            steps {
                echo '🚀 Uploading index.html to Azure Web App via FTP...'
                // Retrieve credentials and assign to environment variables FTP_USER and FTP_PASS
                withCredentials([usernamePassword(credentialsId: 'azure-ftp-creds', usernameVariable: 'FTP_USER', passwordVariable: 'FTP_PASS')]) {
                    script {
                        // Create the full authentication string (username:password) for curl
                        // This handles the Azure username format (appname\$) correctly via Groovy interpolation
                        def ftpAuth = "${FTP_USER}:${FTP_PASS}" 
                        
                        if (isUnix()) {
                            // Unix/Linux: Standard curl syntax with verbose flag
                            sh "curl -v -T index.html -u ${ftpAuth} ftp://${FTP_SITE}${FTP_PATH}"
                        } else {
                            // Windows (bat): Removed single quotes. 
                            // The Groovy string interpolation handles the complex username, 
                            // and removing quotes prevents them from being sent to the FTP server.
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

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
                        def ftpAuth = "${FTP_USER}:${FTP_PASS}" 
                        
                        if (isUnix()) {
                            // Unix/Linux: Use sh step and Groovy string interpolation
                            sh "curl -v -T index.html -u ${ftpAuth} ftp://${FTP_SITE}${FTP_PATH}"
                        } else {
                            // Windows (bat): Use Groovy interpolation and single quotes around auth
                            // This ensures the backslash in the username (appname\$) is passed correctly
                            // to curl without Windows cmd interpreting it first.
                            bat "curl -v -T index.html -u '${ftpAuth}' ftp://${FTP_SITE}${FTP_PATH}"
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

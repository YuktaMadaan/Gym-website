pipeline {
    agent any

    environment {
        // USE THIS EXACT HOSTNAME, based on your latest input
        FTP_SITE = 'waws-prod-pn1-021.ftp.azurewebsites.windows.net' 
        FTP_PATH = '/site/wwwroot/'
    }

    stages {
        // ... (Checkout stage is fine)

        stage('Deploy to Azure') {
            steps {
                echo '🚀 Uploading index.html to Azure Web App via FTP...'
                withCredentials([usernamePassword(credentialsId: 'azure-ftp-creds', usernameVariable: 'FTP_USER', passwordVariable: 'FTP_PASS')]) {
                    script {
                        def ftpAuth = "${FTP_USER}:${FTP_PASS}" 
                        
                        if (isUnix()) {
                            // Unix/Linux: Use sh step
                            sh "curl -v -T index.html -u ${ftpAuth} ftp://${FTP_SITE}${FTP_PATH}"
                        } else {
                            // Windows (bat): Correct syntax for complex username (Gymweb\$)
                            bat "curl -v -T index.html -u ${ftpAuth} ftp://${FTP_SITE}${FTP_PATH}" 
                        }
                    }
                }
            }
        }
    }
    // ... (post block is fine)
}

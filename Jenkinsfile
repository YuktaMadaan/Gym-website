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
                            // Unix/Linux command remains correct
                            sh "curl -v -T index.html -u $FTP_USER:$FTP_PASS ftp://$FTP_SITE$FTP_PATH"
                        } else {
                            // Windows bat command: FTP_USER must be processed by Jenkins to escape the '\'
                            // Use the syntax with the escaped backslash directly
                            def windowsFtpUser = env.FTP_USER.replace('\\', '\\\\') 
                            bat "curl -v -T index.html -u %FTP_USER%:%FTP_PASS% ftp://%FTP_SITE%%FTP_PATH%"
                            // OR, to be safer and more verbose:
                            // The original command is actually correct if the environment variable itself is correctly set by Jenkins:
                            // bat "curl -v -T index.html -u %FTP_USER%:%FTP_PASS% ftp://%FTP_SITE%%FTP_PATH%" 
                            // *If* the resolution issue persists, try the verbose flag and check network (as previously advised).
                            // Let's stick with the original but add -v for diagnostics.
                            bat "curl -v -T index.html -u %FTP_USER%:%FTP_PASS% ftp://%FTP_SITE%%FTP_PATH%"
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

pipeline {
    agent any

    environment {
        // ✅ Azure Web App FTP path
        AZURE_FTP_URL = 'ftp://gym-website-aqehe4d6apa9f5eb.scm.azurewebsites.net/site/wwwroot/'
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo '🔄 Cloning Gym Website from GitHub...'
                git branch: 'main', url: 'https://github.com/YuktaMadaan/Gym-website.git'
            }
        }

        stage('Deploy to Azure') {
            steps {
                echo '🚀 Uploading index.html to Azure Web App...'
                withCredentials([usernamePassword(credentialsId: 'azure-ftp', usernameVariable: 'AZ_USER', passwordVariable: 'AZ_PASS')]) {
                    script {
                        if (isUnix()) {
                            sh "curl -T index.html ${AZURE_FTP_URL} --user ${AZ_USER}:${AZ_PASS}"
                        } else {
                            bat "curl -T index.html ${AZURE_FTP_URL} --user ${AZ_USER}:${AZ_PASS}"
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful! Visit your site at: https://gym-website-aqehe4d6apa9f5eb.eastasia-01.azurewebsites.net'
        }
        failure {
            echo '❌ Deployment failed. Please check Jenkins logs or Azure App Service logs for more details.'
        }
    }
}

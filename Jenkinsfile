pipeline {
    agent any

    environment {
        // ✅ Your Azure FTP endpoint
        AZURE_FTP_URL = 'ftp://gym-website-aqehe4d6apa9f5eb.scm.azurewebsites.net/site/wwwroot/'
    }

    stages {
        stage('Checkout') {
            steps {
                echo '🔄 Pulling latest code from GitHub...'
                git branch: 'main', url: 'https://github.com/YuktaMadaan/Gym-website.git'
            }
        }

        stage('Package Website') {
            steps {
                echo '📦 Zipping Gym Website files...'
                script {
                    if (isUnix()) {
                        sh 'zip -r gym-website.zip *'
                    } else {
                        bat 'powershell Compress-Archive -Path * -DestinationPath gym-website.zip -Force'
                    }
                }
            }
        }

        stage('Deploy to Azure') {
            steps {
                echo '🚀 Deploying Gym Website to Azure Web App...'
                withCredentials([usernamePassword(credentialsId: 'azure-ftp', usernameVariable: 'AZ_USER', passwordVariable: 'AZ_PASS')]) {
                    script {
                        if (isUnix()) {
                            sh "curl -T gym-website.zip ${AZURE_FTP_URL} --user ${AZ_USER}:${AZ_PASS}"
                        } else {
                            bat "curl -T gym-website.zip ${AZURE_FTP_URL} --user ${AZ_USER}:${AZ_PASS}"
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
            echo '❌ Deployment failed. Please check Jenkins logs or Azure App Logs for details.'
        }
    }
}

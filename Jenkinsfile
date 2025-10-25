pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Pulling code from GitHub...'
                git branch: 'main', url: 'https://github.com/YuktaMadaan/Gym-website.git'
            }
        }

        stage('Package') {
            steps {
                echo 'Zipping static website files...'
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
                echo 'Deploying to Azure Web App...'
                // Replace with your actual deployment command
                // Example using FTP:
                script {
                    if (isUnix()) {
                        sh 'curl -T gym-website.zip ftp://<azure-site>.scm.azurewebsites.net/site/wwwroot/ --user <username>:<password>'
                    } else {
                        bat 'curl -T gym-website.zip ftp://<azure-site>.scm.azurewebsites.net/site/wwwroot/ --user <username>:<password>'
                    }
                }
            }
        }
    }
}

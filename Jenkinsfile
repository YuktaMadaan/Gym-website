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
                sh 'zip -r gymwebsite.zip * -x .git/* Jenkinsfile'
            }
        }

        stage('Deploy to Azure') {
            environment {
                AZURE_CREDS = credentials('azure-ftp-creds')
            }
            steps {
                echo "Deploying website to Azure..."
                sh '''
                curl -X POST -u $AZURE_CREDS_USR:$AZURE_CREDS_PSW \
                  --data-binary @gymwebsite.zip \
                  https://gym-website.scm.azurewebsites.net/api/zipdeploy
                '''
            }
        }
    }
}

pipeline {
    agent any

    environment {
        NVM_DIR = "$HOME/.nvm"
        NODE_VERSION = "16"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git(
                    branch: 'master',
                    url: 'https://github.com/rmadan0401/feelio.git',
                    credentialsId: 'github-credentials'
                )
            }
        }

        stage('Setup Node & Install Packages') {
            steps {
                sh '''
                    export NVM_DIR="$HOME/.nvm"
                    [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
                    nvm install $NODE_VERSION
                    nvm use $NODE_VERSION
                    export PATH="$NVM_DIR/versions/node/v$NODE_VERSION.*/bin:$PATH"
                    node -v
                    npm -v
                    npm install
                '''
            }
        }

        stage('Build APK') {
            steps {
                sh '''
                    export NVM_DIR="$HOME/.nvm"
                    [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
                    nvm use $NODE_VERSION
                    export PATH="$NVM_DIR/versions/node/v$NODE_VERSION.*/bin:$PATH"
                    npx expo run:android --variant release
                '''
            }
        }

        stage('Archive APK') {
            steps {
                // Path where APK is generated can vary, usually in android/app/build/outputs/apk
                // We'll use wildcard to find APK
                sh 'find . -name "*.apk"'
                archiveArtifacts artifacts: '**/*.apk', fingerprint: true
            }
        }
    }
}

pipeline {
    agent any

    environment {
        JAVA_HOME = "${env.HOME}/jdk-11.0.2"
        PATH = "${env.JAVA_HOME}/bin:${env.HOME}/gradle-8.2.1/bin:${env.HOME}/android-sdk/cmdline-tools/latest/bin:${env.HOME}/android-sdk/platform-tools:${env.PATH}:${env.HOME}/.npm-global/bin"
        ANDROID_SDK_ROOT = "${env.HOME}/android-sdk"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/rmadan0401/feelio.git', branch: 'main'

            }
        }

        stage('Set Node & NPM') {
            steps {
                // Install Node (no sudo way) & Expo CLI
                sh '''
                    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
                    export NVM_DIR="$HOME/.nvm"
                    source $NVM_DIR/nvm.sh
                    nvm install 16
                    nvm use 16
                    npm config set prefix $HOME/.npm-global
                    export PATH=$HOME/.npm-global/bin:$PATH
                    npm install -g expo-cli
                    npm install
                '''
            }
        }

        stage('Build APK') {
            steps {
                sh '''
                    export NVM_DIR="$HOME/.nvm"
                    source $NVM_DIR/nvm.sh
                    nvm use 16
                    npx expo run:android --variant release
                '''
            }
        }

        stage('Archive APK') {
            steps {
                archiveArtifacts artifacts: '**/app/build/outputs/**/*.apk', fingerprint: true
            }
        }
    }
}

pipeline {
    agent any

    environment {
        NVM_DIR = "/var/lib/jenkins/.nvm"
        NODE_VERSION = "v16.20.2"
        ANDROID_HOME = "/opt/android-sdk"
        ANDROID_SDK_ROOT = "/opt/android-sdk"
        PATH = "/var/lib/jenkins/.nvm/versions/node/${NODE_VERSION}/bin:${ANDROID_HOME}/platform-tools:${ANDROID_HOME}/tools:${ANDROID_HOME}/cmdline-tools/latest/bin:$PATH"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-credentials',
                    url: 'https://github.com/rmadan0401/feelio.git'
            }
        }

        stage('Install Node Modules') {
            steps {
                sh '''
                    export NVM_DIR=/var/lib/jenkins/.nvm
                    [ -s "$NVM_DIR/nvm.sh" ] && \\. "$NVM_DIR/nvm.sh"
                    nvm use ${NODE_VERSION}
                    
                    node -v
                    npm -v
                    npm install
                '''
            }
        }

        stage('Build Android Release APK') {
            steps {
                sh '''
                    export NVM_DIR=/var/lib/jenkins/.nvm
                    [ -s "$NVM_DIR/nvm.sh" ] && \\. "$NVM_DIR/nvm.sh"
                    nvm use ${NODE_VERSION}
                    
                    export ANDROID_HOME=/opt/android-sdk
                    export ANDROID_SDK_ROOT=/opt/android-sdk
                    export PATH=/var/lib/jenkins/.nvm/versions/node/${NODE_VERSION}/bin:$ANDROID_HOME/platform-tools:$ANDROID_HOME/tools:$ANDROID_HOME/cmdline-tools/latest/bin:$PATH
                    
                    sdkmanager --version
                    
                    cd android
                    chmod +x gradlew
                    ./gradlew assembleRelease
                '''
            }
        }

        stage('Archive APK') {
            steps {
                archiveArtifacts artifacts: 'android/app/build/outputs/apk/release/*.apk', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "✅ Build Successful & APK archived!"
        }
        failure {
            echo "❌ Build failed!"
        }
    }
}

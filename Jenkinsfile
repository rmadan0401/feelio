pipeline {
    agent any

    environment {
        NVM_DIR = "/var/lib/jenkins/.nvm"
        NODE_VERSION = "v16.20.2"
        PATH = "/var/lib/jenkins/.nvm/versions/node/${NODE_VERSION}/bin:$PATH"
        ANDROID_HOME = "/opt/android-sdk"
        ANDROID_SDK_ROOT = "/opt/android-sdk"
        PATH = "${ANDROID_HOME}/platform-tools:${ANDROID_HOME}/tools:${ANDROID_HOME}/cmdline-tools/latest/bin:${PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/main']], 
                    userRemoteConfigs: [[
                        url: 'https://github.com/rmadan0401/feelio.git', 
                        credentialsId: 'github-credentials'
                    ]]
                ])
            }
        }

        stage('Setup Node and Dependencies') {
            steps {
                sh '''
                    # Load NVM and use Node.js
                    export NVM_DIR=/var/lib/jenkins/.nvm
                    [ -s "$NVM_DIR/nvm.sh" ] && \\. "$NVM_DIR/nvm.sh"
                    nvm use ${NODE_VERSION}
                    
                    # Verify Node & npm
                    node -v
                    npm -v
                    
                    # Install dependencies
                    npm install
                '''
            }
        }

        stage('Build React Native Android App') {
            steps {
                sh '''
                    # Load NVM
                    export NVM_DIR=/var/lib/jenkins/.nvm
                    [ -s "$NVM_DIR/nvm.sh" ] && \\. "$NVM_DIR/nvm.sh"
                    nvm use ${NODE_VERSION}
                    
                    # Android SDK Environment
                    export ANDROID_HOME=/opt/android-sdk
                    export ANDROID_SDK_ROOT=/opt/android-sdk
                    export PATH=$ANDROID_HOME/platform-tools:$ANDROID_HOME/tools:$ANDROID_HOME/cmdline-tools/latest/bin:$PATH
                    
                    # Verify Android SDK path
                    echo "ANDROID_HOME=$ANDROID_HOME"
                    sdkmanager --version
                    
                    # Gradle permissions
                    chmod +x android/gradlew
                    
                    # Build APK
                    cd android
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
            echo "✅ Build & APK Archive Successful!"
        }
        failure {
            echo "❌ Build Failed!"
        }
    }
}

pipeline {
    agent any

    environment {
        // Java & Android SDK paths
        JAVA_HOME = "/usr/lib/jvm/java-11-openjdk-amd64"
        ANDROID_HOME = "/opt/android-sdk"

        // Adding to PATH
        PATH = "$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/platform-tools:/opt/gradle/gradle-8.5/bin"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Permission Fix') {
            steps {
                echo 'Fixing gradlew permissions...'
                dir('android') {
                    sh 'chmod +x gradlew'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing npm dependencies...'
                sh 'npm install'
            }
        }

        stage('Build Android App') {
            steps {
                echo 'Building Android APK...'
                dir('android') {
                    sh './gradlew assembleDebug'
                }
            }
        }

        stage('Archive APK') {
            steps {
                echo 'Archiving generated APK...'
                dir('android/app/build/outputs/apk/debug') {
                    archiveArtifacts artifacts: '*.apk', fingerprint: true
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build Successful!'
        }
        failure {
            echo '❌ Build Failed. Check logs!'
        }
    }
}

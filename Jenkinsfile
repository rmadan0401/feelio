pipeline {
    agent any

    environment {
        ANDROID_HOME = "/home/ext_rmadan_vecv_in/android-sdk"
        PATH = "${PATH}:${ANDROID_HOME}/platform-tools:${ANDROID_HOME}/emulator"
        NODE_OPTIONS = "--max_old_space_size=4096"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Permission Fix') {
            steps {
                dir('android') {
                    sh 'chmod +x gradlew'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Android App') {
            steps {
                dir('android') {
                    sh './gradlew assembleDebug'
                }
            }
        }

        stage('Archive APK') {
            steps {
                archiveArtifacts artifacts: 'android/app/build/outputs/apk/debug/app-debug.apk', fingerprint: true
            }
        }
    }
}

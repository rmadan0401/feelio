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

        stage('Debug - Current Directory') {
            steps {
                echo 'Printing current directory:'
                sh 'pwd'
                echo 'Listing files:'
                sh 'ls -la'
                echo 'Searching for gradlew file:'
                sh 'find . -name gradlew'
            }
        }

        stage('Permission Fix') {
            steps {
                // Try fixing permissions wherever gradlew is found
                sh 'find . -name gradlew -exec chmod +x {} \\;'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Android App') {
            steps {
                sh 'find . -name gradlew' // Confirm gradlew location again
                // Build from correct directory
                sh 'find . -name gradlew -execdir ./gradlew assembleDebug \\;'
            }
        }

        stage('Archive APK') {
            steps {
                archiveArtifacts artifacts: '**/app/build/outputs/apk/debug/app-debug.apk', fingerprint: true
            }
        }
    }
}

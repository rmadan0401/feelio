pipeline {
    agent any

    environment {
        JAVA_HOME = "${env.HOME}/jdk-11.0.2"  // JDK path you set earlier
        PATH = "${env.JAVA_HOME}/bin:${env.HOME}/gradle-8.2.1/bin:${env.HOME}/android-sdk/cmdline-tools/latest/bin:${env.HOME}/android-sdk/platform-tools:${env.PATH}"
        ANDROID_SDK_ROOT = "${env.HOME}/android-sdk"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/rmadan0401/feelio.git'
            }
        }

        stage('Clean') {
            steps {
                sh './gradlew clean'
            }
        }

        stage('Build APK') {
            steps {
                sh './gradlew assembleDebug'
            }
        }

        stage('Archive APK') {
            steps {
                archiveArtifacts artifacts: '**/app/build/outputs/apk/debug/*.apk', fingerprint: true
            }
        }
    }
}

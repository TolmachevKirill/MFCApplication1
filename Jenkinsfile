pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Clean') {
            steps {
                bat '"C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\MSBuild\\Current\\Bin\\MSBuild.exe" MFCApplication1.sln /t:Clean /p:Configuration=Release /p:Platform="x86"'
            }
        }

        stage('Build') {
            steps {
                bat '"C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\MSBuild\\Current\\Bin\\MSBuild.exe" MFCApplication1.sln /p:Configuration=Release /p:Platform="x86"'
            }
        }

        stage('Archive Build Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/Release/*.exe', fingerprint: true
            }
        }

        stage('Package') {
            steps {
                script {
                    def exePath = "${WORKSPACE}\\Release\\TestMFCApp.exe"
                    bat "\"C:\\Program Files (x86)\\WiX Toolset v3.14\\bin\\candle.exe\" -arch x86 -dMyApplicationExePath=${exePath} installer.wxs -o installer.wixobj"
                    bat "\"C:\\Program Files (x86)\\WiX Toolset v3.14\\bin\\light.exe\" installer.wixobj -o TestMFCApp.msi"
                }
            }
        }

        stage('Save Package Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/*.msi', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Build was successful!'
        }
        failure {
            echo 'Build failed.'
        }
        always {
            echo 'This will always run regardless of build success.'
        }
    }
}

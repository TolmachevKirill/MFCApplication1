pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'msbuild MFCApplication1.sln /p:Configuration=Release /p:Platform="x86"'
            }
        }

        stage('Package') {
            steps {
                script {
                    def exePath = 'C:\\MFCApplication1\\bin\\Release\\TestMFCApp.exe'
                    bat "candle -arch x86 -dMyApplicationExePath=${exePath} installer.wxs -o installer.wixobj"
                    bat "light installer.wixobj -o TestMFCApp.msi"
                }
            }
        }
    }

    post {
        always {
            // Действия, которые должны выполняться после каждой сборки, независимо от её результата
        }
    }
}

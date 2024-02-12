pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Получение исходного кода из системы контроля версий
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Компиляция проекта с помощью MSBuild
                bat '"C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\MSBuild\\Current\\Bin\\MSBuild.exe" MFCApplication1.sln /p:Configuration=Release /p:Platform="x86"'
            }
        }

        stage('Package') {
            steps {
                // Указание на скомпилированный .exe для создания MSI с помощью WiX
                script {
                    def exePath = 'C:\\MFCApplication1\\bin\\Release\\TestMFCApp.exe'
                    bat "candle -arch x86 -dMyApplicationExePath=${exePath} installer.wxs -o installer.wixobj"
                    bat "light installer.wixobj -o TestMFCApp.msi"
                }
            }
        }
    }

    post {
        success {
            // Действия после успешной сборки
            echo 'Build was successful!'
        }
        failure {
            // Действия после неудачной попытки сборки
            echo 'Build failed.'
        }
        always {
            // Добавьте сюда шаги, которые должны выполняться после каждого выполнения пайплайна
            echo 'This will always run regardless of build success.'
        }
    }
}

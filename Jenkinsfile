pipeline {
    agent any

    environment {
        // Здесь можно указать переменные окружения, например пути к компиляторам или версиям инструментов
    }

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
                bat 'msbuild MFCApplication1.sln /p:Configuration=Release /p:Platform="x86"'
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

        // Здесь могут быть дополнительные этапы, например, тестирование или развертывание
    }

    post {
        success {
            // Действия после успешной сборки
        }
        failure {
            // Действия после неудачной попытки сборки
        }
    }
}

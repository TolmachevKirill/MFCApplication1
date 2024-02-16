pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Получение исходного кода из системы контроля версий
                checkout scm
            }
        }

        stage('Clean') {
            steps {
                // Очистка предыдущих артефактов сборки
                bat '"C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\MSBuild\\Current\\Bin\\MSBuild.exe" MFCApplication1.sln /t:Clean /p:Configuration=Release /p:Platform="x86"'
            }
        }

        stage('Build') {
            steps {
                // Компиляция проекта с помощью MSBuild
                bat '"C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\MSBuild\\Current\\Bin\\MSBuild.exe" MFCApplication1.sln /p:Configuration=Release /p:Platform="x86"'
            }
        }

        stage('Archive Build Artifacts') {
            steps {
                // Сохранение артефактов сборки (например, .exe файла) для последующего использования
                archiveArtifacts artifacts: '**/Release/*.exe', fingerprint: true
            }
        }

        stage('Package') {
            steps {
                // Упаковка в MSI с использованием WiX
                script {
                    def exePath = 'Release\\TestMFCApp.exe'
                    // Использование установленных версий WiX Toolset
                    bat '"C:\\Program Files (x86)\\WiX Toolset v3.14\\bin\\candle.exe" -arch x86 -dMyApplicationExePath=${exePath} installer.wxs -o installer.wixobj'
                    bat '"C:\\Program Files (x86)\\WiX Toolset v3.14\\bin\\light.exe" installer.wixobj -o TestMFCApp.msi'
                }
            }
        }

        stage('Save Package Artifacts') {
            steps {
                // Сохранение артефактов упаковки (MSI файла)
                archiveArtifacts artifacts: '**/*.msi', fingerprint: true
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
            // Действия, которые должны выполняться после каждого выполнения пайплайна
            echo 'This will always run regardless of build success.'
        }
    }
}

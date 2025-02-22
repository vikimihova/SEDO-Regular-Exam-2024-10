pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Setup .NET') {
            steps {
                script {
                    def dotnetVersion = '6.0.x'
                    bat "curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin --version $dotnetVersion"
                    bat 'export PATH="$PATH:$HOME/.dotnet"'
                    bat 'dotnet --version'
                }
            }
        }
        stage('Restore dependencies') {
            steps {
                bat 'dotnet restore'
            }
        }
        stage('Build') {
            steps {
                bat 'dotnet build --no-restore'
            }
        }
        stage('Run tests') {
            steps {
                bat 'dotnet test --no-build --verbosity normal'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/*.xml', allowEmptyArchive: true
            junit 'results/**/*.xml'
        }
    }
}

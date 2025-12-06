pipeline {
    agent any

    stages {
        stage('Checkout code') {
            steps {
                git branch: 'main', url: 'https://github.com/yankoov/SeleniumIdeJenkinsWorkshop2.git'
            }
        }

        stage('Setup .net') {
            steps {
                script {
                    bat """
                    echo Setting up .NET 8.0 SDK
                    choco install dotnet-8.0-sdk -y
                    """
                }
            }
        }

        stage('Restore dependencies') {
            steps {
                script {
                    bat """
                    echo Restoring .NET dependencies
                    dotnet restore
                    """
                }
            }
        }

        stage('Build project') {
            steps {
                script {
                    bat """
                    echo Building the .NET project
                    dotnet build --configuration Debug
                    """
                }
            }
        }

        stage('Run tests') {
            steps {
                script {
                    bat """
                    echo Running tests
                    dotnet test SeleniumIde.sln --logger "trx;LogFileName=SeleniumIDE/TestResults/test_results.trx"
                    """
                }
            }
        }
    }

    post {
        always {
            echo "Archiving test results..."

            archiveArtifacts artifacts: 'SeleniumIDE/TestResults/test_results.trx', allowEmptyArchive: true
            
            junit 'SeleniumIDE/TestResults/test_results.trx'
        }
    }
}

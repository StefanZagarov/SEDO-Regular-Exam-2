pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *')
    }

    environment {
        DOTNET_ROOT = "${WORKSPACE}/.dotnet"
        PATH = "${WORKSPACE}/.dotnet:${PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install .NET 8 SDK') {
            steps {
                sh '''
                    rm -rf "${WORKSPACE}/.dotnet"
                    
                    curl -sSL https://dot.net/v1/dotnet-install.sh -o dotnet-install.sh
                    chmod +x dotnet-install.sh
                    
                    ./dotnet-install.sh --version 8.0.100 --install-dir "${WORKSPACE}/.dotnet"
                    
                    "${WORKSPACE}/.dotnet/dotnet" --version
                    "${WORKSPACE}/.dotnet/dotnet" --list-runtimes
                '''
            }
        }
        stage('Restore') {
            steps {
                sh '"${WORKSPACE}/.dotnet/dotnet" restore'
            }
        }
        stage('Build') {
            steps {
                sh '"${WORKSPACE}/.dotnet/dotnet" build --no-restore'
            }
        }      
        stage('Test') {
            steps {
                sh '"${WORKSPACE}/.dotnet/dotnet" test --no-build --verbosity normal'
            }
        }
    }
}

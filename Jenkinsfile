pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                sh 'git clone https://github.com/Emre-Igten/p3ops-demo-app.git'
            }
        }

        stage('Build and Test in dotnet6-container') {
            environment {
                DOTNET_IMAGE = 'mcr.microsoft.com/dotnet/sdk:6.0'
            }
            steps {
                sh "docker run --rm --name dotnet6-container -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker -v ${PWD}:/workspace -w /workspace $DOTNET_IMAGE /bin/bash -c 'dotnet build p3ops-demo-app/src/Server/Server.csproj && dotnet test p3ops-demo-app/tests/Domain/Domain.csproj'"
            }
        }

        stage('Deploy to Test Environment') {
            steps {
                sh 'docker stop sportstore-container || true'
                sh 'docker rm sportstore-container || true'
                sh 'docker run -d --name sportstore-container -e DOTNET_ENVIRONMENT=Production -e DOTNET_ConnectionStrings__SqlDatabase="Server=sql-server-container;Database=SportStore;User Id=SA;Password=AVeryComplex123Password;MultipleActiveResultSets=true" dotnet6-container'
            }
        }
    }
}

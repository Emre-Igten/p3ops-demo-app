pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                script {
                    // Cloning the repository in dotnet6-container
                    docker.image('mcr.microsoft.com/dotnet/sdk:6.0').inside('-u root') {
                        sh 'git clone https://github.com/Emre-Igten/p3ops-demo-app.git'
                    }
                }
            }
        }

        stage('Build and Test in dotnet6-container') {
            steps {
                script {
                    // Execute build and test commands in existing dotnet6-container
                    docker.image('mcr.microsoft.com/dotnet/sdk:6.0').inside {
                        sh 'dotnet build p3ops-demo-app/src/Server/Server.csproj'
                        sh 'dotnet test p3ops-demo-app/tests/Domain/Domain.csproj'
                    }
                }
            }
        }

        stage('Deploy to Test Environment') {
            steps {
                script {
                    // Stop and remove previous container if it exists
                docker.image('mcr.microsoft.com/dotnet/sdk:6.0').inside {
                    sh 'docker stop sportstore-container || true'
                    sh 'docker rm sportstore-container || true'

                    // Run the existing SportStore .NET app container
                    sh 'docker run -d --name sportstore-container -e DOTNET_ENVIRONMENT=Production -e DOTNET_ConnectionStrings__SqlDatabase="Server=sql-server-container;Database=SportStore;User Id=SA;Password=AVeryComplex123Password;MultipleActiveResultSets=true" dotnet6-container'
                }
            }
        }
    }
}

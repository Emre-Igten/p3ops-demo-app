pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Emre-Igten/p3ops-demo-app.git'
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    // Build .NET solution
                    sh 'dotnet build src/Server/Server.csproj'

                    // Run unit tests for the domain
                    sh 'dotnet test tests/Domain/Domain.csproj'
                }
            }
        }

        stage('Deploy to Test Environment') {
            steps {
                script {
                    // Stop and remove previous container if exists
                    sh 'docker stop sportstore-container || true'
                    sh 'docker rm sportstore-container || true'

                    // Run the existing SportStore .NET app container
                    sh 'docker run -d --name sportstore-container -e DOTNET_ENVIRONMENT=Production -e DOTNET_ConnectionStrings__SqlDatabase="Server=sql-server-container;Database=SportStore;User Id=SA;Password=AVeryComplex123Password;MultipleActiveResultSets=true" dotnet6-container'
                }
            }
        }
    }
}

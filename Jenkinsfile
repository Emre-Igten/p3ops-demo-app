pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                script {
                    // Cloning the repository in dotnet6-container
                    sh 'docker exec dotnet6-container git clone https://github.com/Emre-Igten/p3ops-demo-app.git'
                }
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    // Execute build and test commands in existing dotnet6-container
                    sh 'docker exec dotnet6-container dotnet build src/Server/Server.csproj'
                    sh 'docker exec dotnet6-container dotnet test tests/Domain/Domain.csproj'
                }
            }
        }

        stage('Deploy to Test Environment') {
            steps {
                script {
                    // Stop and remove previous container if it exists
                    sh 'docker stop sportstore-container || true'
                    sh 'docker rm sportstore-container || true'

                    // Run the existing SportStore .NET app container
                    sh 'docker run -d --name sportstore-container -e DOTNET_ENVIRONMENT=Production -e DOTNET_ConnectionStrings__SqlDatabase="Server=sql-server-container;Database=SportStore;User Id=SA;Password=AVeryComplex123Password;MultipleActiveResultSets=true" dotnet6-container'
                }
            }
        }
    }
}

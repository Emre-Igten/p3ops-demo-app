pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                sh 'git clone https://github.com/Emre-Igten/p3ops-demo-app.git'
            }
        }
        // stage('Pull Dotnet Image') {
        //     steps {
        //         script {
        //             sh 'docker pull mcr.microsoft.com/dotnet/sdk:6.0'
        //         }
        //     }
        // }        
        stage('Copy files to dotnet6-container') {
            steps {
                script {
                    // Copy a file
                    sh 'docker cp /var/jenkins_home/workspace/Testing/p3ops-demo-app dotnet6-container:'
                }
            }
        }

        stage('Build and Test in dotnet6-container') {
            steps {
                        sh 'docker exec dotnet6-container dotnet build src/Server/Server.csproj'
                        sh 'docker exec dotnet6-container dotnet test tests/Domain.Tests/Domain.Tests.csproj'
                    
                }
            }
        

        stage('Deploy to Test Environment') {
            steps {
                script {
                    // Stop and remove previous container if it exists
                    sh 'docker stop sportstore-container || true'
                    sh 'docker rm sportstore-container || true'

                    // Run the existing SportStore .NET app container
                    sh 'docker run -d --name sportstore-container \
                        -e DOTNET_ENVIRONMENT=Production \
                        -e DOTNET_ConnectionStrings__SqlDatabase="Server=sql-server-container;Database=SportStore;User Id=SA;Password=AVeryComplex123Password;MultipleActiveResultSets=true" \
                        dotnet6-container'
                }
            }
        }
    }
}



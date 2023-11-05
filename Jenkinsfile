pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/dotnet/sdk:6.0'
        }
    }

    stages {
        stage('Git Checkout') {
            steps {
                sh 'git clone https://github.com/Emre-Igten/p3ops-demo-app.git'
            }
        }

        stage('Build and Test in dotnet6-container') {
            steps {
                        sh 'dotnet build p3ops-demo-app/src/Server/Server.csproj'
                        sh 'dotnet test p3ops-demo-app/tests/Domain/Domain.csproj'
                   
                }
            }
        }

        stage('Deploy to Test Environment') {
            steps {

                        sh 'docker stop sportstore-test-container || true'
                        sh 'docker rm sportstore-test-container || true'

                        
                        sh 'docker run -d --name sportstore-test-container -e DOTNET_ENVIRONMENT=Production -e DOTNET_ConnectionStrings__SqlDatabase="Server=sql-server-container;Database=SportStore;User Id=SA;Password=AVeryComplex123Password;MultipleActiveResultSets=true" dotnet6-container'
                    
                }
            }
        }
    


pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                sh 'git clone https://github.com/Emre-Igten/p3ops-demo-app.git'
            }
        }

        stage('Build and Test in dotnetTEST-container') {
            environment {
                DOTNET_IMAGE = 'mcr.microsoft.com/dotnet/sdk:6.0'
            }
            steps {
                sh "docker run --rm --name dotnetTEST-container \
                    -v /var/run/docker.sock:/var/run/docker.sock \
                    -v \$(which docker):/usr/bin/docker \
                    -v \${PWD}:/workspace -w /workspace \
                    -v /var/jenkins_home/workspace/p3ops-demo-app:/workspace/p3ops-demo-app \
                    $DOTNET_IMAGE /bin/bash -c 'dotnet build p3ops-demo-app/src/Server/Server.csproj && \
                    dotnet test p3ops-demo-app/tests/Domain.Tests/Domain.Tests.csproj'"
            }
        }

        stage('Deploy to Test Environment') {
            steps {
                sh 'docker stop sportstore-container || true'
                sh 'docker rm sportstore-container || true'
                sh 'docker run -d --name sportstore-container \
                    -e DOTNET_ENVIRONMENT=Production \
                    -e DOTNET_ConnectionStrings__SqlDatabase="Server=sql-server-container;Database=SportStore;User Id=SA;Password=AVeryComplex123Password;MultipleActiveResultSets=true" \
                    dotnetTEST-container'
            }
        }
    }
}

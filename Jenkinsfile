pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                sh 'git clone https://github.com/Emre-Igten/p3ops-demo-app.git'
            }
        }
      
        stage('Copy files to dotnet6-container') {
            steps {
                script {
                    // Copy a file
                    sh 'docker cp /var/jenkins_home/workspace/Testing/p3ops-demo-app dotnet6-container:'
                }
            }
        }

        stage('Delete REPO') {
            steps {
                sh 'if [ -d "/var/jenkins_home/workspace/Testing/" ]; then rm -Rf /var/jenkins_home/workspace/Testing/; fi'
            }
        }

        stage('Build and Test in dotnet6-container') {
            steps {
                        sh 'docker exec dotnet6-container dotnet build src/Server/Server.csproj'
                        sh 'docker exec dotnet6-container dotnet test tests/Domain.Tests/Domain.Tests.csproj'
                    
                }
            }
        
        stage('Publish and start the test app if it works') {
            steps {
                script { 
                    sh 'docker exec dotnet6-container dotnet publish src/Server/Server.csproj -c Release -o publish'
                    sh 'docker exec dotnet6-container bash -c "cd publish && dotnet Server.dll"'
                }
            }
        }
    }
}



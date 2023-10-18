
## TESTING


### VM OPSTARTEN, NODIGE INSTALLATIES UITVOEREN, DOCKER RUNNEN MET JENKINS

- vagrant up
- vagrant ssh control
- ansible-galaxy install -r requirements.yml
- ssh key toevoegen aan known hosts   
  -> in inventory.yml 'ansible_ssh_pass: vagrant' verwijderen en dan 'ansible-playbook -i inventory.yml docker.yml' uitvoeren  
  -> 'yes' bij fingerprint  
  ->  'ansible_ssh_pass: vagrant' terug toevoegen in inventory en dan weer 'ansible-playbook -i inventory.yml docker.yml' uitvoeren  
- ansible-playbook -i inventory.yml testdocker.yml
- ansible-playbook -i inventory.yml jenkinsrunnen.yml 
- op de vm    
  -> sudo usermod -aG docker $USER  
  -> sudo systemctl restart docker  
  -> sudo docker run -p 8080:8080 -u root \
          -v jenkins-data:/var/jenkins_home \  
          -v $(which docker):/usr/bin/docker \  
          -v /var/run/docker.sock:/var/run/docker.sock \  
          -v "$HOME":/home \  
          --name jenkins_server jenkins/jenkins:lts
- surfen naar http://192.168.56.2:8080/
- inloggen met wachtwoord  
  -> docker exec -it jenkins_server /bin/cat /var/jenkins_home/secrets/initialAdminPassword


### SQL IN DOCKER

- ansible-playbook -i inventory.yml sqltest.yml
- op de vm  
  -> docker run --name SQLCONT -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=st3rkw8w00rd!" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2022-latest
  -> docker exec -it SQLCONT /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P st3rkw8w00rd!  
- zorg dat firewall alles toelaat 
  -> sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent  
  -> sudo firewall-cmd --reload




  ### andere zaken


 - SQL en DOTNET cont in zelfde docker netwerk plaatsen  
   -> docker network create netwerknaam  
   -> docker network connect [netwerknaam] [container-naam-of-ID]
 - docker cp src dotnet6-container:/app -> app naar docker dotner cont verplaatsen


 ## applicatie bouwen in docker met dotnet


- docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=st3rkw8w00rd!" -p 1433:1433 --name SQLServerContainer -d mcr.microsoft.com/mssql/server:2022-latest

- docker run -e "DOTNET_ENVIRONMENT=Production" -e "DOTNET_ConnectionStrings__SqlDatabase=Server=localhost;Database=SportStore;User Id=SA;Password=A!VeryComplex123Password;MultipleActiveResultSets=true" -p 5000:80 --name DotnetContainer -d mcr.microsoft.com/dotnet/sdk:6.0
- docker cp src DotnetContainer:
- docker exec -it DotnetContainer /bin/bash 
- cd src
- dotnet restore src/Server/Server.csproj
- dotnet build src/Server/Server.csproj
- dotnet publish src/Server/Server.csproj -c Release -o publish









- sudo docker run -it \
    -e "ACCEPT_EULA=Y" \
    -e "SA_PASSWORD=A&VeryComplex123Password" \
    -p 1433:1433 \
    --name sql-server-container \
    mcr.microsoft.com/mssql/server:2019-latest

- 
  

docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=A&VeryComplex123Password" -p 1433:1433 --name sql_server_container -d mcr.microsoft.com/mssql/server:2019-latest


"SqlDatabase": "Server=localhost;Database=SportStore;User Id=SA;Password=A!VeryComplex123Password;MultipleActiveResultSets=true"




{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    },
    "ConnectionStrings": {
      "SqlDatabase": "Server=sql_server2022;Database=SportStore;User Id=SA;Password=A!VeryComplex123Password;MultipleActiveResultSets=true"
    }
  },
  "AllowedHosts": "*"
}







  




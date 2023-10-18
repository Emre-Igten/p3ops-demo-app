
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



  -> sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent  
  -> sudo firewall-cmd --zone=public --add-port=5000/tcp --permanent  
  -> sudo firewall-cmd --reload




### DOCKER

 - docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=AVeryComplex123Password" -p 1433:1433 --name SQLServerContainer -d mcr.microsoft.com/mssql/server:2022-latest  
 - ansible-playbook -i inventory.yml dotnetDOCKER.yml  
 - SQL en DOTNET cont in zelfde docker netwerk plaatsen,     
   -> docker network create proofofconcept  
   -> docker network connect proofofconcept SQLServerContainer  
   -> docker network connect proofofconcept dotnet6-containerTEST2  



-> docker exec -it dotnet6-containerTEST2 bash  
-> dotnet restore src/Server/Server.csproj  
-> dotnet build src/Server/Server.csproj  
-> export DOTNET_ConnectionStrings__SqlDatabase="Server=SQLServerContainer;Database=SportStore;User Id=SA;Password=AVeryComplex123Password;MultipleActiveResultSets=true"  
-> export DOTNET_ENVIRONMENT=Production  
-> cd /p3ops-demo-app/publish# dotnet Server.dll



## ALS CONNECTION NIET LUKT BIJ src/Server/Program.cs -> webBuilder.UseUrls("http://0.0.0.0:5000");















- sudo dnf update  
- sudo dnf install ansible  
- kijken of je alma kan pingen met 'ansible -i inventory.yml alma -m ping'    
  -> ansible-galaxy install -r requirements.yml  
- als dit lukt 'ansible-playbook -i inventory.yml docker_net.yml' uitvoeren
- ansible-playbook -i inventory.yml testdocker.yml
- ansible-playbook -i inventory.yml jenkinsrunnen.yml


## TESTING


### VM OPSTARTEN, NODIGE INSTALLATIES UITVOEREN, DOCKER RUNNEN MET JENKINS

- vagrant up
- vagrant ssh control
- ansible-galaxy install -r requirements.yml
- ssh key toevoegen aan known hosts   
  -> in inventory.yml 'ansible_ssh_pass: vagrant' verwijderen en dan 'ansible-playbook -i inventory.yml docker_net.yml' uitvoeren  
  -> 'yes' bij fingerprint  
  ->  'ansible_ssh_pass: vagrant' terug toevoegen in inventory en dan weer 'ansible-playbook -i inventory.yml docker_net.yml' uitvoeren  
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
  -> docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=st3rkw8w00rd!" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2022-latest
  -> docker exec -it funny_golick /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P st3rkw8w00rd!  
  ->   
     CREATE DATABASE SportStore;  
     GO;  
     EXIT;
- zorg dat firewall alles toelaat 
  -> sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent  
  -> sudo firewall-cmd --reload

  Data Source=192.168.56.2;Initial Catalog=SportStore;User ID=sa;Password=***********

  Data Source=host.docker.internal,1433;Initial Catalog=SportStore;User ID=sa;Password=st3rkw8w00rd!









  




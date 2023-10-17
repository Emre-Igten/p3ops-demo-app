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
  -> docker run -d -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=testdb' -p 1433:1433 --name sql_
server mcr.microsoft.com/mssql/server:latest


### .NET RUNNEN MET TOOLS


- demo app kopieren naar vm met 'scp -r ...'  
- in de directory van de app  
    -> git config --global user.email "emre.igten@student.hogent.be"  
    -> git config --global user.name "Emre Igten"  
    -> git init  
    -> git add .    
    -> git commit -m "Initial commit of sample application"  
    -> git remote add origin git@github.com:Emre-Igten/p3ops-demo-app.git
- 




  




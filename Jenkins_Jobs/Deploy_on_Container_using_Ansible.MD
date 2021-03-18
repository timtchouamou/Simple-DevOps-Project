# Deploy on a docker container using Ansible
# *Jenkins Job name:* `Deploy_on_Container_using_ansible`

### Pre-requisites

1. Jenkins server 
1. Docker-host server 
1. Ansible server

*`/opt/docker`* will be our workplace to store artifacts, so create if doesn't exist
   chown ansadmin:ansadmin /opt/docker

### Integration between Ansible-control-node and Jenkins

Install "publish Over SSH"
 - `Manage Jenkins` > `Manage Plugins` > `Available` > `Publish over SSH`

Enable connection between Ansible-control-node and Jenkins

- `Manage Jenkins` > `Configure System` > `Publish Over SSH` > `SSH Servers` 

	- SSH Servers:
                - Name: `ansible-server`
		- Hostname:`<ServerIP>`
		- username: `ansadmin`
               
       -  `Advanced` > chose `Use password authentication, or use a different key`
		 - password: `*******`
   
 
### Steps to create "Deploy_on_Container_using_ansible" Jenkin job
#### From Jenkins home page select "New Item"
   - Enter an item name: `Deploy_on_Container_using_ansible`
     - Copy from: `Deploy_on_Container`
     
   - *Source Code Management:*
      - Repository: `https://github.com/yankils/hello-world.git`
      - Branches to build : `*/master`  
   - *Poll SCM* :      - `* * * *`

   - *Build:*
     - Root POM:`pom.xml`
     - Goals and options: `clean install package`

 - *Post-build Actions*
   - Send build artifacts over SSH
     - *SSH Publishers*
      - SSH Server Name: `ansible-server`
       - `Transfers` >  `Transfer set`
            - Source files: `webapp/target/*.war`
	       - Remove prefix: `webapp/target`
	       - Remote directory: `//opt//docker`
	       - Exec command: 
                
Test first without any exec command

Save and run the job now.

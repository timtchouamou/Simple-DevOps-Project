# Deploy on a docker container using Jenkins 
# `Deploy_on_Container`

### Pre-requisites

1. Jenkins server 
2. Docker-host server 
3. `Dockerfile` under *`/home/dockeradmin`* in user home directory on docker host 
   ```sh 
   # Pull tomcat latest image from dockerhub 
   From tomcat: 9
   # Maintainer
   MAINTAINER "Tim tchouamou" 

   # copy war file on to container 
   COPY ./webapp.war /usr/local/tomcat/webapps
   
### Integration between Docker-host and Jenkins

Install "publish Over SSH"
 - `Manage Jenkins` > `Manage Plugins` > `Available` > `Publish over SSH`

Enable connection between Docker-host and Jenkins

- `Manage Jenkins` > `Configure System` > `Publish Over SSH` > `SSH Servers` 

	- SSH Servers:
                - Name: `docker-host`
		- Hostname:`<ServerIP>`
		- username: `dockeradmin`
               
       -  `Advanced` > chose `Use password authentication, or use a different key`
		 - password: `*******`
 
### Steps to create "Deploy_on_Container" Jenkin job
#### From Jenkins home page select "New Item"
   - Enter an item name: `Deploy_on_Container`
     - Copy from: `Deploy_on_Docker_Host`
     
   - *Source Code Management:*
      - Repository: `https://github.com/itresolve/hello-world.git`
      - Branches to build : `*/master`  
 

   - *Build:*
     - Root POM:`pom.xml`
     - Goals and options: `clean install package`

 - *Post-build Actions*
   - Send build artifacts over SSH
     - *SSH Publishers*
      - SSH Server Name: `docker-host`
       - `Transfers` >  `Transfer set`
            - Source files: `webapp/target/*.war`
	       - Remove prefix: `webapp/target`
	       - Remote directory: `.`
	       - Exec command: 
                ```sh 
                cd /home/dockeradmin;
                docker build -t devops-image .; 
                docker run -d --name devops-container -p 8080:8080 devops-image;
                ```
		
Build the job and make sure first you don't have any container running with that name

### If you try to rebuilt a new code it will fail AND this is what we will solve next with Ansible.
### Also asume your are deploying on multiple system, you must registe each system to jenkins which is a lot to do, ANSIBLE make it easy
Save and run the job now.

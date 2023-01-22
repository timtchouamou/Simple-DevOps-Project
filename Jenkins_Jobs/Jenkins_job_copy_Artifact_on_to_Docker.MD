# Deploy on Docker host server using Jenkins 
# *Jenkins Job name:* `Deploy_on_Docker_Host`

### Pre-requisites

1. Jenkins server 
2. Docker-host Server 
3. Integration between Docker-host and Jenkins done

### First integrate Docker with Jenkins
Go to Jenkins console and install the plugin “Publish Over SSH”
Go to Manage Jenkins --- configure system, go down to publish over ssh and click on add  (provide the name, hostname or IP, username is dockeradmin and for password check password auth box to enter pass)

 
### Steps to create "Deploy_on_Docker_Host" Jenkin job
 #### From Jenkins home page select "New Item"
   - Enter an item name: `Deploy_on_Docker_Host`
     - Copy from: `Deploy_on_Tomcat_Server`
     
   - *Source Code Management:*
      - Repository: `https://github.com/itresolve/hello-world.git`
      - Branches to build : `*/master`  
   - *Poll SCM* :      * * * * *`

*Disable POLL SCM*
*Delete deploy to container*
ADD THE FOLLOWING

 - *Post-build Actions*
   - Send build artifacts over SSH
     - *SSH Publishers*
      - SSH Server Name: `docker-host`
       - `Transfers` >  `Transfer set`
            - Source files: `webapp/target/*.war`     `SEE in jenkins server /var/lib/jenkins/workplace`
	       - Remove prefix: `webapp/target`
	       - Remote directory: `.`
	 
BUILD and go to docker-host to verify dockeradmin home

Save and run the job now.

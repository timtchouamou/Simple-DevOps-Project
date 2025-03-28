# Deploy on a Tomcat server
# *Jenkins Job name:* `Deploy_on_Tomcat_Server`

### Pre-requisites

1. Jenkins server 
2. Tomcat Server 

### Adding Deployment steps

1. Install 'deploy to container' plugin. This plugin needs to be deploy on Jenkins server. 

  - Install 'deploy to container' plugin without restart  
    - `Manage Jenkins` > `Plugins` > `available` > `deploy to container`
 
2. Jenkins should need access to the tomcat server to deploy build artifacts. setup credentials to enable this process. use credentials option on Jenkins home page.

### Steps to create "Deploy_on_Tomcat_Server" Jenkin job
 #### From Jenkins home page select "New Item"
   - Enter an item name: `Deploy_on_Tomcat_Server`
     - Copy From : My_First_Maven_Build
     
   - *Source Code Management:*
      - Repository: `https://github.com/timtchouamou/hello-world.git`
      - Branches to build : `*/master`  
   - *Poll SCM* :      - `* * * *`

   - *Build:*
     - Root POM:`pom.xml`
     - Goals and options: `clean install package`

 - *Post-build Actions*
   - Deploy war/ear to container
      - WAR/EAR files : `**/*.war`
      - Containers : `Tomcat 9.x`
         - Credentials:
               Username	: `deployer`
               Password : `1qaz2wsx`
               id      :  `deployer-user`
               Description: `user to deploy on tomcat vm`
         - Tomcat URL : `http://<PUBLIC_IP>:8090`

### Apply and Save, run the job now.
verify 
cd /opt/tomcat/webapps    to see webapp.war  just got created
http://<PUBLIC_IP>:8080/webapp

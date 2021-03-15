
1. `Dockerfile` under *`/opt/docker`* on Ansible server
   ```sh 
   # Pull tomcat latest image from dockerhub 
   From tomcat
   # Maintainer
   MAINTAINER "ItResolveIt" 

   # copy war file on to container 
   COPY ./webapp.war /usr/local/tomcat/webapps
   
   
1. Create `simple-devops-image.yml` under *`/opt/docker`* on Ansible server 
   ```sh
   ---
   - hosts: all
     #ansadmin doesn't need root access to create an image
     become: true 

     tasks:
     - name: building docker image using war file
       command: "docker build -t simple-devops-image ." 
       args:
         chdir: /opt/docker
   ```
   
   TEST TO MAKE SURE IT WORKS BEFORE YOUR ADD THE NEXT ONE (Update the inventory file)
   
1. Create container `simple-devops-container.yml` under the above 
   ```ssh
   ---
   - hosts: all
     become: true
     
     tasks:
     - name: creating docker container using docker command
       command: docker run -d --name simple-devops-container -p 8080:8080 simple-devops-image
   ```
   
   TEST TO MAKE SURE IT WORKS AGAIN

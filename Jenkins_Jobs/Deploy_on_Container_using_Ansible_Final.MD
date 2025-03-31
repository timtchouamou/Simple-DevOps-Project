
1. `Dockerfile` under *`/opt/docker`* on Ansible server
   ```sh 
   # Pull tomcat latest image from dockerhub 
   From tomcat
   # Maintainer
   MAINTAINER "tim tchouamou" 

   # copy war file on to container 
   COPY ./webapp.war /usr/local/tomcat/webapps
   
   
1. Create `tomcat-image.yml` under *`/opt/docker`* on Ansible server 
   ```sh
   ---
   - hosts: all
     #ansadmin doesn't need root access to create an image
     become: true 

     tasks:
     - name: building docker image using war file
       command: "docker build -t tomcat-image ." 
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
       command: docker run -d --name tomcat-container -p 8090:8080 tomcat-image
   ```
   
   TEST TO MAKE SURE IT WORKS AGAIN

# Installing Docker on Amazon Linux server

### Pre-requisites
1. Lunch new Amazon Linux EC2 Instance
1. Change hostname to docker-host

## Installation Steps

1. Install docker and start docker services
   ```sh 
   yum install docker -y
   docker --version 
   
   # start docker services
   service docker start
   service docker status
   ```
1. Create a user called dockeradmin
   ```sh
   useradd dockeradmin
   passwd dockeradmin
   ```
1. add a user to docker group to manage docker 
   ```
   usermod -aG docker dockeradmin
   ```
### Validation test
1. Create a tomcat docker container by pulling a docker image from the public docker registry
   ```sh
   docker pull tomcat:latest
   docker run --name test-tomcat-server -p 8090:8080 tomcat:latest
   docker run -d --name test-tomcat-server -p 8090:8080 tomcat:latest
   d = dettach
   ```
   
### Fix HTTP Status 404: NOT FOUND
  ``` sh
    LOGIN TO THE CONTAINER
    docker exec -it <container_name> /bin/bash
    Check to make sure you have ROOT directory under webapps is not copy from webapps.dist
    cp -R webapps.dist/* webapps
    Then refresh the websitet to verify
    
    To avoid this issue, we can use tomcat:8.0 image version for our build
    ```
    





# Tomcat installation on EC2 instance

### Pre-requisites
1. EC2 instance with Java v11.x 
yum install -y java-11*

### Install Apache Tomcat
1. Download tomcat packages from  https://tomcat.apache.org/download-80.cgi onto /opt on EC2 instance
   ```sh 
   Change hpstname to tomcat
   # Create tomcat directory
   cd /opt
   wget https://mirrors.sonic.net/apache/tomcat/tomcat-9/v9.0.41/bin/apache-tomcat-9.0.41.tar.gz
   tar -xvzf /opt/apache-tomcat-9.0.41.tar.gz
   ```
   mv apache-tomcat-9.0.41 tomcat
   
   cd tomcat/bin
 
   ./startup.sh     #To start tomcat
   ```
   
  #### Check point :
access tomcat application from browser on port 8080  
 - http://<Public_IP>:8080

### SKIP: Using unique ports for each application is a best practice in an environment. But tomcat and Jenkins runs on ports number 8080. Hence lets change tomcat port number to 8090. Change port number in conf/server.xml file under tomcat home
   ```sh
 cd /opt/apache-tomcat-8.5.35/conf
# update port number in the "connecter port" field in server.xml
# restart tomcat after configuration update
./shutdown.sh
./startup.sh
```

#### Check point :
Access tomcat application from browser on port 8090  
 - http://<Public_IP>:8080

1. Tomcat application doesnt allow to login from browser. changing a default parameter in context.xml does address this issue  (Try to click on "manage app" from the browser)
   ```sh
   #search for context.xml
   find / -name context.xml      Then edit the following files
   /opt/tomcat/webapps/host-manager/META-INF/context.xml
   /opt/tomcat/webapps/manager/META-INF/context.xml
   ```
1. above command gives 3 context.xml files. comment `(<!-- & -->)` `Value ClassName` field on files which are under webapp directory.
As follow
```
 <!-- <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->
```
After that restart tomcat services to effect these changes
   ```sh 
   ./shutdown.sh
   ./startup.sh
   ```
   
   Try again to open "Manage app"
   
1. Update users information in the tomcat-users.xml file.
Add below users to conf/tomcat-user.xml file just before </tomcat-users> , The last line
   ```sh
	<role rolename="manager-gui"/>
	<role rolename="manager-script"/>
	<role rolename="manager-jmx"/>
	<role rolename="manager-status"/>
	<user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
	<user username="deployer" password="1qaz2wsx" roles="manager-script"/>
	<user username="tomcat" password="1qaz2wsx" roles="manager-gui"/>
   ```
1. Restart serivce and try to login to tomcat application from the browser. This time it should be Successful


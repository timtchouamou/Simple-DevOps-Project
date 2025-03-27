#  Install & configure Maven build tool on Jenkins
Maven is a code build tool which is used to convert your code to an artifact. this is a widely used plugin to build in continuous integration


#### Prerequisites
1. Jenkins server

#### Install Maven on Jenkins
1. Download maven packages onto Jenkins server. In this case, I am using /opt/maven as my installation directory
 - Link : https://maven.apache.org/download.cgi
    ```sh
     cd /opt
     # downloading maven version 3.9.9  and uncompress
     wget https://mirror.jframeworks.com/apache/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz
     tar -xvzf apache-maven-3.9.9-bin.tar.gz
     mv apache-maven-3.9.9 maven
     ```
	
1. Setup M2_HOME and M2 paths in .bash_profile of the user and add these to the path variable
   ```sh
   vi ~/.bash_profile
   M2_HOME=/opt/maven
   M2=/opt/maven/bin
   PATH=<Existing_PATH>:$M2_HOME:$M2
   ```
### Checkpoint 
1. Verify path and check maven version
   ``` sh
    source ~/.bash_profile
    echo $M2
    echo $M2_HOME
    mvn --version
    ```
So far we have completed the installation of maven software to support maven plugin on the jenkins console. Let's jump onto Jenkins to complete the remaining steps. 

### Setup maven on Jenkins console
1. Install maven plugin without restart  
  - `Manage Jenkins` > `Plugins` > `available` > `Maven Invoker`
  - `Manage Jenkins` > `Plugins` > `available` > `Maven Integration`

2. Configure maven path
  - `Manage Jenkins` > `Tool` > `Maven` --> `NAME is M2_HOME    MAVEN HOME is /opt/maven


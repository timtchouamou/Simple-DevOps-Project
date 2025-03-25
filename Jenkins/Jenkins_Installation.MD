# Install Jenkins on AWS EC2
Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Mac OS X and other Unix-like operating systems. As an extensible automation server, Jenkins can be used as a simple CI server or turned into the continuous delivery hub for any project.


### Prerequisites
1. EC2 Instance 
   - With Internet Access
   - Security Group with Port `8080` open for internet
1. Java v1.8.x 

## Install Java
1. We will be using open java for our demo, Get the latest version from http://openjdk.java.net/install/
   ```sh
   yum install java-11* -y or java 17*
  NB: Because of some update jenkins does not support no more less than version 11
   ```

1. Confirm Java Version and set the java home
   ```sh
   java -version
   find /usr/lib/jvm/java-11* | head -n 3   ## to get java version
   vi ~/.bash_profile   ## to update the below permanently to you path
   JAVA_HOME=/usr/lib/jvm/java-11-amazon-corretto.x86_64    <Java version which seen in the above output>
   M2_HOME=/opt/maven
   M2=/opt/maven/bin
   PATH=$PATH:$HOME/bin:$JAVA_HOME:$M2_HOME:$M2
   
   [root@jenkins ~]# source ~/.bash_profile
   
   
   ```
   Verify java is installed well, The output should be something like below.
   
   [root@jenkins ~]# java -version
  openjdk version "11.0.18" 2023-01-17 LTS
OpenJDK Runtime Environment Corretto-11.0.18.10.1 (build 11.0.18+10-LTS)
OpenJDK 64-Bit Server VM Corretto-11.0.18.10.1 (build 11.0.18+10-LTS, mixed mode)
   ```

## Install Jenkins
 You can install jenkins using the rpm or by setting up the repo. We will set up the repo so that we can update it easily in the future.
1. Get the latest version of jenkins from https://pkg.jenkins.io/redhat-stable/ and install
   ```sh
   yum -y install wget
   a) sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins.io/redhat/jenkins.repo
   b) sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io-2023.key
   yum -y install jenkins
   line a$b configure the Jenkins repository
   ```

   ### Start Jenkins and Setup Jenkins to start at boot
   ``` sh
   # systemctl enable --now jenkins


   ### Accessing Jenkins
   By default jenkins runs at port `8080`, You can access jenkins at
   ```sh
   http://YOUR-SERVER-PUBLIC-IP:8080
   ```
   
   
  #### Configure Jenkins
- The default Username is `admin`
- Grab the default password 
- Password Location:`/var/lib/jenkins/secrets/initialAdminPassword`
- `Skip` Plugin Installation; _We can do it later_
- Change admin password
   - `Admin` > `Configure` > `Password`
   
- Configure `java` path
  - `Manage Jenkins` > `Global Tool Configuration` > `JDK` . `Add the NAME = JAVA_HOME  and the link
  `# echo $JAVA_HOME    to get JAVA_HOME path
 

### Test Jenkins Jobs
1. Create “new item”
1. Enter an item name – `My-First-Project`
   - Chose `Freestyle` project
1. Under the Build section
	Execute shell: echo "Welcome to Jenkins Demo"
1. Save your job 
1. Build job
1. Check "console output"

### CONGRATULATION FOR COMPLETING YOUR VERY FIRST JOB ON JENKINS


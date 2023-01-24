# Ansible Installation

Ansible is an open-source automation platform. It is very, very simple to set up and yet powerful. Ansible can help you with configuration management, application deployment, task automation.

### Pre-requisites

1. An AWS EC2 instance (on Control node)

### Installation steps:
#### on Amazon EC2 instance

# Change hostname to control-node or ansible
hostnamectl set-hosname ansible

# Update Amazon Linux2 AM
   yum update

# Install ansible using amazon-linux-extras check for version
    ```sh
   amazon-linux-extras install ansible2
   ansible --version
   ```
   
1. Create a user called ansadmin (on Control node and Docker host)  
   ```sh
   useradd ansadmin
   passwd ansadmin
   ```
1. Give root privilege to ansadmin 

usermod -aG weel ansadmin and edit visudo to ## Same thing without a password
 %wheel ALL=(ALL)       NOPASSWD: ALL
   
1. Log in as a ansadmin user on master and generate ssh key (on Control node)
   ```sh 
   sudo su - ansadmin
   ssh-keygen
   ```
1. Copy keys onto all ansible managed hosts (on Control node)
   ```sh 
   ssh-copy-id ansadmin@<target-server>  Target is docker-host
   ssh-copy-id localhost
   ```

1. Ansible server used to create images and store on docker registry. Hence install docker, start docker services and add ansadmin to the docker group. 
   ```sh
   yum install docker
   
   # start docker services 
   service docker start
   
   # add user to docker group 
   usermod -aG docker ansadmin
   go to /etc/ssh/sshd_config to allow password auth
   ```
1. Create a directory /etc/ansible and create an inventory file called "hosts" add control node and managed hosts IP addresses to it. 
 
 ```sh
 ip-of-docker-host
 localhost
 ```
### Validation test

   
1. Run ansible command as ansadmin user it should be successful (Master)
   ```sh 
   ansible all -m ping
   ```

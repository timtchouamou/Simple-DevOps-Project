# Configure Git pulgin on Jenkins
Git is one of the most popular tools for version control system. you can pull code from git repositories using jenkins if you use github plugin. 


#### Prerequisites
1. Jenkins server 

#### Install Git on Jenkins server
2. Install git packages on jenkins server
   ```sh
   yum install git -y
   ```

#### Setup Git on jenkins console
-3 Install git plugin without restart  
  - `Manage Jenkins` > `Manage Plugins` > `available` > `then search for "github"`

-4 Configure git path
  - `Manage Jenkins` > `Global Tool Configuration` > `git` > `and change NAME from default to github  and path from git to /usr/bin/git


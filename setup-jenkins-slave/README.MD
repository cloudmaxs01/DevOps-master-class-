# Configure Jenkins Slaves on AWS EC2
Jenkins is a free and open source automation server. It is a self-contained Java-based program, It helps automate the parts of software development related to building, testing, and deploying, facilitating continuous integration and continuous delivery

Build >> Deploy >> Test >> Relase

### Follow this article in **[Youtube](https://youtu.be/hwrYURP4O2k)**
![Jenkins Master and Slave Configuration](https://github.com/cloudmaxs01/DevOps-master-class-/blob/master/setup-jenkins-slave/images/Jenkins-Master-and-Slave-Configuration.png)

### Prerequisites
1. Jenkins Master Running 
1. EC2 RHEL 7.x Instance for Slave Node
   - With Internet Access
   - Security Group with Port `8080` open for internet
   - Java v1.8.x 

## Install Java
We will be using open java for our demo, Get latest version from http://openjdk.java.net/install/. Also configure the default `JAVA_HOME` path
```sh
yum install java-1.8*
#yum -y install java-1.8.0-openjdk
```
## Setup Jenkins Slave
```sh
# Create user and add the user to wheel group
useradd jenkins-slave-01
passwd jenkins-slave-01
cd /etc/ssh
ls
vi sshd_config
## uncomment  PasswordAuthentication yes AND comment PasswordAuthentication no
service sshd restart

# Create SSH Keys
sudo su - jenkins-slave-01
ssh-keygen -t rsa -N "" -f /home/jenkins-slave-01/.ssh/id_rsa
# The private and public keys will be created at these locations `/home/jenkins-slave-01/.ssh/id_rsa` and `/home/jenkins-slave-01/.ssh/id_rsa.pub`
cd .ssh
cat id_rsa.pub > authorized_keys
chmod 700 authorized_keys
```

### Configuration on Master
Copy the slave node's public key[`id_rsa.pub`] to Master Node's `known_hosts` file
```sh
mkdir -p /var/lib/jenkins/.ssh
cd /var/lib/jenkins/.ssh
ssh-keyscan -H SLAVE-NODE-IP-OR-HOSTNAME >>/var/lib/jenkins/.ssh/known_hosts
# ssh-keyscan -H 172.31.38.42 >>/var/lib/jenkins/.ssh/known_hosts
chown jenkins:jenkins /var/lib/jenkins/.ssh/known_hosts
chmod 700 /var/lib/jenkins/.ssh/known_hosts
##run the below command to generate your private kay
more id_rsa
```

#### Configure the Slave using `Manage Jenkins`
Configure the node as shown here
`Manage Jenkins` > `Manage Nodes` > `New Node`
## the host should be your pravait Ip address and not the pubilc
![Jenkins Master and Slave Configuration](https://github.com/cloudmaxs01/DevOps-master-class-/blob/master/setup-jenkins-slave/images/Slave-Node-Configuration-01.png)

### Test Jenkins Jobs
1. Create “new item”
1. Enter an item name – `My-First-Project`
   - Chose `Freestyle` project
1. Under `General` Section
   - Choose `Restrict where this project can be run`
     - Update your _jenkins slave label_ `Java` 
1. Under Build section
   Execute shell
   ```sh
   #!/bin/bash
   echo "_______________________________"
   echo "|                             |"
   echo "|   Welcome to my Demo    |"
   echo "|           _nnnn_            |"
   echo "|          dGGGGMMb           |"
   echo "|         @p~qp~~qMb          |"
   echo "|         M|@||@) M|          |"
   echo "|         @,----.JM|          |"
   echo "|        JS^\__/  qKL         |"
   echo "|       dZP        qKRb       |"
   echo "|      dZP          qKKb      |"
   echo "|     fZP            SMMb     |"
   echo "|     HZM            MMMM     |"
   echo "|     FqM            MMMM     |"
   echo "|   __| '.        |\dS'qML    |"
   echo "|   |    '.       | ' \Zq     |"
   echo "|  _)      \.___.,|     .'    |"
   echo "|  \____   )MMMMMP|   .'      |"
   echo "|       '-'       '--' hjm    |"
   echo "_______________________________"
   ```
1. Save your job 
1. Build job
1. Check "console output"

### Other things you need to know about jenkins
 - [x] [Install and Configure Jenkins in AWS EC2]
 - [x] [Configure Users & Groups in Jenkins]
 - [x] [Secure your Jenkins Server]
 - [x] [Jenkins Plugin Installation]
 - [x] [Jenkins Master-Slave Configuration]


# Jenkins Server Setup in Linux VM #

## Step-1: Create Linux VM for Master-Node ##
```
1) Create Ubuntu VM using AWS EC2 (t2.medium)
2) Enable 8080 Port Number in Security Group Inbound Rules
3) Connect to VM using MobaXterm
```
## Step-2: Installation of Java ##

```
sudo apt update -y
sudo apt install fontconfig openjdk-17-jre -y
java -version
```

## Step-3: Setting JAVA_HOME and Path Environment Variables ##
```
ls /usr/lib/jvm/
echo "export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64" >> ~/.bashrc
echo "export PATH=\$JAVA_HOME/bin:\$PATH" >> ~/.bashrc
echo $JAVA_HOME
```

## Step-4: Verify the Java Installation ##
```
java -version
```

## Step-5: Installation of Jenkins ##
```
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update -y
sudo apt-get install jenkins -y
```

## Step-6: Start Jenkins ## 

```
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

## Step-7: Verify Jenkins ##

```
sudo systemctl status jenkins
```
## Step-8: Open Jenkins server in browser using VM public ip ##

```
http://public-ip:8080/
```

## Step-9: Copy Jenkins admin Password ##
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
	   
## Step-10: Create Admin Account & Install Required Plugins in Jenkins ##
```
Name: xxxxxx
username: xxxxxx
password: xxxxxx
confirm password: xxxxxx
email-id: xxxxx@gmail.com
```
# Prepare the Slave Node #

## Step-11: Create Linux VM for Slave-Node ##
```
1) Create Ubuntu VM using AWS EC2 (t2.micro)
2) Enable 22 Port Number in Security Group Inbound Rules
3) Connect to VM using MobaXterm
```
## Step-12: Installation of Java ##

```
sudo apt update -y
sudo apt install fontconfig openjdk-17-jre -y
java -version
```

## Step-13: Setting JAVA_HOME and Path Environment Variables ##
```
ls /usr/lib/jvm/
echo "export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64" >> ~/.bashrc
echo "export PATH=\$JAVA_HOME/bin:\$PATH" >> ~/.bashrc
echo $JAVA_HOME
```

## Step-14: Verify the Java Installation ##
```
java -version
```

## Step-15: Installation of ssh ##

```
sudo apt install ssh

```
## Step-16: Switch to the Super-user ##

```
sudo su -

```
## Step-17: Create the Jenkins user ##

```
useradd -m jenkins -s /bin/bash

```
## Step-18: Switch to the Jenkins-user ##

```
su - jenkins

```
## Step-19: Generate the ssh key ##

```
ssh-keygen

```
## Step-20: Copy the public_key into authorized_keys ##

```
cat id_rsa.pub > authorized_keys

```
## Step-21: Configure Jenkins Master ##

```
1) Open Jenkins Dashboard: Go to your Jenkins instance in your web browser.
2) Manage Nodes:
    a) Click on Manage Jenkins from the left-hand side menu.
    b) Click on Manage Nodes and Clouds.
    c) Click on New Node

```
## Step-22: Add a New Node ##

```
1) Node Configuration:
    a) Enter a name for the node (e.g., slave-node-1).
    b) Select Permanent Agent.
Click OK.
2) Configure Node Details:
    a) Remote root directory: Specify the directory on the slave node where Jenkins will store files (e.g., /home/jenkins).
    b) Labels: Add any labels you want to use to identify this node.
    c) Usage: Choose Use this node as much as possible.
    d) Launch method: Select Launch agents via SSH.

```
## Step-23: Configure SSH Connection ##

```
1) Host: Enter the IP address or hostname of the slave node.
2) Credentials: Add SSH credentials (username = jenkins and private key = copy from slave node)
4) Host Key Verification Strategy: Choose Manual trusted key Verification Strategy
5) Tick on Require manual verification of initial connection

```
## Step-24: Trsut the Node ##

```
In Dashboard itself you can see and click on Trust SSH Host key

```
## Step-25: Verify Node Connection ##

```
Check Node Status: Go to Manage Jenkins > Manage Nodes and Clouds and verify that the new node is online.
```
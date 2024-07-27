# Jenkins Server Setup in Linux VM #

## Step-1 : Create Linux VM ##

1) Create Ubuntu VM using AWS EC2 (t2.medium)
2) Enable 8080 Port Number in Security Group Inbound Rules
3) Connect to VM using MobaXterm

## Step-2 : Installation of Java ##

```
sudo apt update -y
sudo apt install fontconfig openjdk-17-jre -y
java -version
```

## Step-3 Setting JAVA_HOME and Path Environment Variables ##
```
ls /usr/lib/jvm/
echo "export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64" >> ~/.bashrc
echo "export PATH=\$JAVA_HOME/bin:\$PATH" >> ~/.bashrc
echo $JAVA_HOME
```

## Step-4 Verify the Java Installation ##
```
java -version
```

## Step-5 : Installation of Jenkins ##
```
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update -y
sudo apt-get install jenkins -y
```

## Step-6 : Start Jenkins ## 

```
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

## Step-7 : Verify Jenkins ##

```
sudo systemctl status jenkins
```
## Step-8 : Open Jenkins server in browser using VM public ip ##

```
http://public-ip:8080/
```

## Step-9 : Copy Jenkins admin Password ##
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
	   
## Step-10 : Create Admin Account & Install Required Plugins in Jenkins ##
```
Name: xxxxxx
username: xxxxxx
password: xxxxxx
confirm password: xxxxxx
email-id: xxxxx@gmail.com
```

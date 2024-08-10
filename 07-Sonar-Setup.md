# Sonar Server Setup in Linux VM #

# Step-1: Create Linux VM for Master-Node #
```
1) Create Ubuntu VM using AWS EC2 (t2.medium)
2) Enable 9000 Port Number in Security Group Inbound Rules
3) Connect to VM using MobaXterm
```
# Step-2: Firstly we need to upgrade and update the packages #

```
sudo apt update
sudo apt upgrade -y
```

# Step-3: Effortless Installation: Java and PostgreSQL Made Simple! #

## Step-3(a): To unlock the full potential of the latest SonarQube version, ensure you have OpenJDK 17 installed ##

```
sudo apt update -y
sudo apt install fontconfig openjdk-17-jre -y
java -version
```

## Step-3(b): Setting JAVA_HOME and Path Environment Variables ##
```
ls /usr/lib/jvm/
echo "export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64" >> ~/.bashrc
echo "export PATH=\$JAVA_HOME/bin:\$PATH" >> ~/.bashrc
echo $JAVA_HOME
```

## Step-3(c): Verify the Java Installation ##
```
java -version
```

## Step-3(d): To install and configure PostgreSQL, start by adding the PostgreSQL repository ##
```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" /etc/apt/sources.list.d/pgdg.list'
```

## Step-3(e): To proceed with installing and configuring PostgreSQL, add the PostgreSQL signing key ## 

```
wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -
```

## Step-3(f): To complete the PostgreSQL setup, proceed by installing PostgreSQL ##

```
sudo apt install postgresql postgresql-contrib -y
```
## Step-3(g): Ensure the database server starts automatically upon reboot for seamless operation ##

```
sudo systemctl enable postgresql
```

## Step-3(h): Initiate the database server to begin operations ##
```
sudo systemctl start postgresql
```
	   
## Step-3(i): Verify the current status of the database server ##
```
sudo systemctl status postgresql
```

## Step-3(j): It’s crucial to validate the installed version of the Postgres database ##
```
psql --version
```
## Step-3(k): Switch to the Postgres user account for further configuration ##

```
sudo -i -u postgres
```

# Step-4: Database Mastery: Crafting Configurations with Elegance! #

## Step-4(a): Create a new database user to manage the sqube database ##
```
createuser sona
```

## Step-4(b): Log in to the PostgreSQL database to proceed with database operations ##
```
psql
```

## Step-4(c): Set a strong password for the “sona” user. Use a password that meets security requirements in place of `my_password`##

```
ALTER USER sona WITH ENCRYPTED password 'Sona#123';

```
## Step-4(d): Create a sqube database and assign the ownership to the “sona” user ##

```
CREATE DATABASE sqube OWNER sona;

```
## Step-4(e): Grant all privileges on the “sqube” database to the “sona” user ##

```
GRANT ALL PRIVILEGES ON DATABASE sqube to sona;

```
## Step-4(f): To verify the creation of the database, use the following command ##

```
\l

```
## Step-4(g): To verify the creation of the database user, use the following command ##

```
\du

```
## Step-4(h): To exit the PostgreSQL command-line interface, use the following command ##

```
\q

```
## Step-4(i): To return to your non-root sudo user account, use the following command ##

```
exit

```
# Step-5: Downloading and Installing SonarQube #
## Step-5(a):  Install the zip utility, required for extracting the SonarQube files, using the following command ##

```
sudo apt install zip -y

```
## Step-5(b): we are installing the latest version of SonarQube 10.4 Community Edition (free version) ##

```
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.4.1.88267.zip
```
## Step-5(c): Unzip the downloaded file using the following command:##

```
sudo unzip sonarqube-10.4.1.88267.zip

```
## Step-5(d): Move the unzipped files to the /opt/sonarqube directory using the following command ##

```
sudo mv sonarqube-10.4.1.88267 sonarqube
sudo mv sonarqube /opt/
```

# Step-6: Setting Up SonarQube: Adding Group and User#
## Step-6(a): Create a dedicated user and group for SonarQube ##
```
sudo groupadd sona
sudo useradd -d /opt/sonarqube -g sona sona
```
## Step-6(b): Grant the “sona” user access to the /opt/sonarqube directory using the following command ##
```
sudo chown -R sona:sona /opt/sonarqube
```
# Step-7: Fine-Tuning SonarQube: Configuring for Peak Performance #
## Step-7(a): Edit the SonarQube configuration file located at /opt/sonarqube/conf/sonar.properties ##
```
sudo nano /opt/sonarqube/conf/sonar.properties
```

## Step-7(b): Uncomment the following lines in the SonarQube configuration file ##
```
sonar.jdbc.username=sona
sonar.jdbc.password=Sona#123
```
## Step-7(c): Below these lines, add the following line to specify the JDBC URL ##
```
sonar.jdbc.url=jdbc:postgresql://localhost:5432/sqube

```
![logo](https://github.com/ravisrma/DevOps-tools-Installation/blob/main/Image3.png)
## Step-7(d):  Replace “sona” with your database username, “Sona#123” with your database password, and “sqube” with your database name##
## Step-7(e): Save and exit the sonar.properties file ##
## Step-7(f): To edit the SonarQube script file, use the following command ##
```
sudo nano /opt/sonarqube/bin/linux-x86-64/sonar.sh
```
## Step-7(g): Replace “linux-x86–64” with the appropriate directory for your installation if it’s different ##
## Step-7(h): Add the following line to the SonarQube script file (/opt/sonarqube/bin/linux-x86–64/sonar.sh) to specify the user to run SonarQube as ##
```
RUN_AS_USER=sona
```
![logo](https://github.com/ravisrma/DevOps-tools-Installation/blob/main/Image2.png)
## Step-7(i): Replace “sona” with the name of the user you created earlier ##
## Step-7(j): Save and exit the sonar.sh file ##
# Step-8: Streamlining SonarQube: Setting Up a systemd Service#
## Step-8(a): To create a systemd service file for SonarQube, use the following steps ##
```
(i) Create a new service file using a text editor:
    sudo nano /etc/systemd/system/sonar.service
(ii) Add the following content to the file:
    [Unit]
    Description=SonarQube service
    After=syslog.target network.target
    [Service]
    Type=forking
    ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
    ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop
    User=sona
    Group=sona
    Restart=always
    LimitNOFILE=65536
    LimitNPROC=4096
    [Install]
    WantedBy=multi-user.target
Make sure to replace “User=sona” and “Group=sona” with the appropriate values you have created for your SonarQube installation.
(iii)Save and close the file.
```
## Step-8(b): Reload the systemd sonar to apply the changes ##
```
sudo systemctl enable sonar
```
## Step-8(c): Enable the SonarQube service to start at boot##
```
sudo systemctl start sonar
```
## Step-8(d): Now, you can start the SonarQube service using the following command##
```
sudo systemctl status sonar
```
## Step-8(e): Hooray, it’s up and running smoothly! ##

# Step-9: Enhancing Performance: Modifying Kernel System Limits##

## Step-9(a): To ensure SonarQube functions correctly with Elasticsearch, you’ll need to make some adjustments to the system defaults##

## Step-9(b): To edit the sysctl configuration file, use the following command:##
```
sudo nano /etc/sysctl.conf
```
## Step-9(c): This file controls various kernel parameters##

## Step-9(d): Add the following lines to the sysctl configuration file (/etc/sysctl.conf)##
```
vm.max_map_count=262144
fs.file-max=65536
ulimit -n 65536
ulimit -u 4096
```

![logo](https://github.com/ravisrma/DevOps-tools-Installation/blob/main/Image1.png)

## Step-9(e): Save and exit the sysctl configuration file##

## Step-9(f): To apply the changes, reboot the system using the following command##
```
sudo reboot
```

# Step-10: Exploring SonarQube: Accessing the Web Interface#

## Step-10(a): Access SonarQube in a web browser by entering your server’s IP address followed by port 9000##
```
For example, http://IP:9000.
```
## Step-10(b): The default username and password are both “admin”##

## Step-10(c): To change the password in SonarQube, follow these steps##
```
(i) Log in to SonarQube using the username “admin” and password “admin”.

(ii) Once logged in, SonarQube will prompt you to change your password. Enter the current password “admin” and then enter your new password twice as prompted.

(iii) Click on the “Change password” button to save your new password.

Your password has now been successfully changed.
```
## Step-10(c): Congratulations on successfully installing SonarQube Community version 10.0 on AWS EC2 Ubuntu 22! If you have any more questions or need further assistance, feel free to ask. Happy coding!##

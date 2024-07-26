# Docker Server Setup in Linux VM #

## Step-1: Create Linux VM ##

1) Create Ubuntu VM using AWS EC2 (t2.micro)
2) Connect to VM using MobaXterm

## Step-2: Install Java In Ubuntu VM ##
```
sudo apt-get update  
sudo apt install default-jdk -y
java --version
``` 

## Step-3: Create a System User ##
```
sudo useradd -m -d /opt/tomcat -U -s /bin/false tomcat
```

## Step-4: Install Tomcat ##
```
wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.26/bin/apache-tomcat-10.1.26.tar.gz -O /tmp/tomcat-10.tar.gz
sudo -u tomcat tar -xzvf /tmp/tomcat-10.tar.gz --strip-components=1 -C /opt/tomcat
```

## Step-5: Create a Systemd Service File for Tomcat ##
```
[Unit]
Description=Apache Tomcat
After=network.target

[Service]
Type=forking

User=tomcat
Group=tomcat

Environment=JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
Environment=CATALINA_PID=/opt/tomcat/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

ExecReload=/bin/kill $MAINPID
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

## Step-6: Save the file and exit ##
```
:wq
```

## Step-7: Reload the systemd manager configuration ##
```
sudo systemctl daemon-reload
```

## Step-8: To run Tomcat now and make the service run upon reboot ##
```
sudo systemctl enable --now tomcat
```

## Step-9: Verify Tomcat service ##
```
sudo systemctl status tomcat
```

## Step-10: Open your web browser ##
```
http://YOUR_SERVER_IP_ADDRESS:8080 
```

## Step-11: To remove the restriction on the Host Manager ##
```
Path: vi /opt/tomcat/webapps/manager/META-INF/context.xml

<Valve className="org.apache.catalina.valves.RemoteAddrValve"
allow=".*" />
```

## Step-12: Configure Tomcat ##
```
Path: vi /opt/tomcat/conf/tomcat-users.xml

<role rolename="manager-gui" />
<user username="tomcat" password="tomcat" roles="manager-gui"/>
<role rolename="admin-gui" />
<user username="admin" password="admin" roles="manager-gui,admin-gui"/>
```

## Step-13: Restart Tomcat ##
```
cd /bin

./shutdown.sh
./startup.sh
```
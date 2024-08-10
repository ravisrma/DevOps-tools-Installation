# Maven Setup in Linux VM #

## Step-1 : Create Linux VM ##

1) Create Ubuntu VM using AWS EC2 (t2.micro)
2) Connect to VM using MobaXterm

## Step-2 Install java 17 ##
```
sudo apt update
sudo apt install openjdk-17-jdk -y
``` 

## Step-3 Setting JAVA_HOME and Path Environment Variables ##
```
echo "export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64" >> ~/.bashrc
echo "export PATH=\$JAVA_HOME/bin:\$PATH" >> ~/.bashrc
source ~/.bashrc
```

## Step-4 Verify the Java Installation ##
```
java -version
```

## Step-5 Download the Maven Binaries ##
```
wget https://dlcdn.apache.org/maven/maven-3/3.9.8/binaries/apache-maven-3.9.8-bin.tar.gz
tar -xvf apache-maven-3.9.8-bin.tar.gz
mv apache-maven-3.9.8 /opt/
```

## Step-6 Setting M2_HOME and Path Variables ##
```
M2_HOME='/opt/apache-maven-3.9.8'
PATH="$M2_HOME/bin:$PATH"
export PATH
```

## Step-7 Verify the Maven installation ##
```
mvn -version
```
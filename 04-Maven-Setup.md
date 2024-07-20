# Maven Setup in Linux VM #

## Step-1 : Create Linux VM ##

1) Create Ubuntu VM using AWS EC2 (t2.micro)
2) Connect to VM using MobaXterm

## Step-2 Download the JDK Binaries ##
```
wget https://download.java.net/java/GA/jdk13.0.1/cec27d702aa74d5a8630c65ae61e4305/9/GPL/openjdk-13.0.1_linux-x64_bin.tar.gz
tar -xvf openjdk-13.0.1_linux-x64_bin.tar.gz
mv jdk-13.0.1 /opt/
``` 

## Step-3 Setting JAVA_HOME and Path Environment Variables ##
```
JAVA_HOME='/opt/jdk-13.0.1'
PATH="$JAVA_HOME/bin:$PATH"
export PATH
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

## Step-6 Verify the Maven installation ##
```
mvn -version
```
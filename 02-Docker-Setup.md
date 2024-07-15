# Docker Server Setup in Linux VM #

## Step-1 : Create Linux VM ##

1) Create Ubuntu VM using AWS EC2 (t2.micro)
2) Connect to VM using MobaXterm

## Step-2 Install Docker In Ubuntu VM ##
```
sudo apt-get update  
sudo apt-get install ca-certificates curl  
sudo install -m 0755 -d /etc/apt/keyrings  
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc  
sudo chmod a+r /etc/apt/keyrings/docker.asc  
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null  
sudo apt-get update 
``` 

## Step-3 Install the Docker packages ##
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Step-4 Verify docker installation ##
```
docker --version
```

## Step-5 To run Docker without root privileges ##
```
sudo usermod -aG docker $USER
```

## Step-6 Test the docker ##
```
docker run hello-world
```
## Install Docker
```sh
yum install docker
systemctl start docker
systemctl enable docker
```

## provide permissions to jenkins user in jenkins server to access docker
* cat /etc/group/ | grep -i docker  *#will be created by default*
* cat /etc/group/ | grep -i jenkins  *#will be created by default*
```sh
  sudo groupadd docker
  sudo usermod -aG docker jenkins
  sudo chmod 777 /var/run/docker.sock
```
## Add Jenkins user into sudoers file to get sudo access
```sh
   vi /etc/sudoers OR visudo
   jenkins ALL=(ALL) NOPASSWD: ALL
```   
#
### For UBUNTU

1. sudo nano docker.sh

```sh
#!/usr/bin/env bash
sudo apt update -y
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" -y
apt-cache policy docker-ce
sudo apt install docker-ce -y
sudo systemctl status docker
sudo usermod -aG docker ${USER}
```

2. bash docker.sh
 
OR

 chmod +x docker.sh
 ./docker.sh

3. systemctl start docker
4. systemctl enable docker / chkconfig docker on


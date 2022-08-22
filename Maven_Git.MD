# Install and Configure Maven & git in Jenkins server

## Install Maven
```sh
cd /opt/
yum install wget
wget https://dlcdn.apache.org/maven/maven-3/3.8.6/binaries/apache-maven-3.8.6-bin.tar.gz
# >>> Download the latest maven @ https://maven.apache.org/download.cgi
tar -xvzf apache-maven-3.8.6-bin.tar.gz

1. cd .. OR ~ (home)
2. vim .bash_profile
 >> export M2_HOME=/opt/apache-maven-3.8.6
 >> export M2=$M2_HOME/bin
 >> PATH=$PATH:$M2
# To set it permanently update your .bash_profile
source ~/.bash_profile

**Validate Maven**

mvn version
```

## Install git *(It is needed for Jenkins Pipeline purpose)*
```sh
yum install git
```

## Assign shell to jenkins user

```sh
vi /etc/passwd
change shell from /bin/false to /bin/bash under Jenkins
```

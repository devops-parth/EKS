# EKS CI/CD Pipeline
EKS CI/CD Pipeline using Jenkins, Docker, Helm, Kubernetes etc.

1. Launch EC2 :
	* **t2.medium**
	* Amazon Linux *(my case)*
	* Security Group: Inbound Rule= TCP port 22, 8080
2. Terminal
	* sudo su -
	* Install OpenJDK 1.8
	* ls -ltra >> .bash_profile >> JAVA_HOME
	* [Jenkins Installation](https://github.com/devops-parth/EKS/blob/master/Jenkins_Installation.MD)
	* service jenkins status/start
	* chkconfig jenkins on *#same as enabling*
	* netstat -ntlp *#:::8080 used by Java*
	* Login to Jenkins:8080 and cat the Password
    * [Maven and Git Installation](https://github.com/devops-parth/EKS/blob/master/Maven_Git.MD)
	* Install Maven >> ls -ltra >> .bash_profile >> $M2
	* [Docker Installation](https://github.com/devops-parth/EKS/blob/master/Docker.MD)
3. Create EKS Cluster
	* **t2.micro**
	* Amazon Linux *(my case)*
	* Security Group: Inbound Rule= TCP port 22

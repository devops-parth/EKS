
$$
EKS ~CI/CD ~Pipeline
$$

EKS CI/CD Pipeline using Jenkins, Docker, Helm, Kubernetes etc.

1. Launch EC2 :
	* **t2.medium** for **Jenkins**
	* **t2.micro** for **Kubernetes**
	* Amazon Linux *(my case)*
	* Security Group: Inbound Rule= TCP port 22, 8080
2. Install **Jenkins**
	* sudo su -
	* Install OpenJDK 1.8
	* ls -ltra >> .bash_profile >> JAVA_HOME
	* [Jenkins Installation](https://github.com/devops-parth/EKS/blob/master/Jenkins_Installation.MD)
	* service jenkins status/start
	* chkconfig jenkins on *#same as enabling*
	* netstat -ntlp *#:::8080 used by Java*
	* Login to Jenkins:8080 and cat the Password
3. Install **Maven** & **GIT**
	*  [Maven and Git Installation](https://github.com/devops-parth/EKS/blob/master/Maven_Git.MD)
	* Install Maven >> ls -ltra >> .bash_profile >> $M2
4. Install **Docker**
	* [Docker Installation](https://github.com/devops-parth/EKS/blob/master/Docker.MD)
5. Create **EKS Cluster** in  **Kubernetes Server**
	* **t2.micro**
	* Amazon Linux *(my case)*
	* Security Group: Inbound Rule= TCP port 22
	* [EKS Cluster Installation](https://github.com/devops-parth/EKS/blob/master/EKS_SetupAWS.MD)
6. Install & Configure **HELM**(Version 3) in **Jenkins Server**
	* Attach the same IAM Role to Jenkins Server that was used in K8s Management Server
	* [HELM Installation](https://github.com/devops-parth/EKS/blob/master/Jenkins_HELM.MD)
	* [Stable HELM Charts](https://github.com/devops-parth/EKS/blob/master/HELM_Commands.MD)
	> The deployed HELM chart works because we have copied the config file from .kube of Kubernetes and pasted in Jenkins Server. Also we have given IAM role to communicate with EC2 to Jenkins Server
	* [Stable HELM Charts](https://github.com/devops-parth/EKS/blob/master/HELM_Commands.MD)
	* [Create Custom Helm Chart](https://github.com/devops-parth/EKS/blob/master/HELM_CustomChart.MD)
		* [Helm Files & Jenkinsfile](https://github.com/devops-parth/EKS/tree/master/Helm%20Files%20%26%20Jenkinsfile)
	> Create Custom Chart and use 
	*


$$
\Huge EKS ~CI/CD ~Pipeline
$$

$$
\bigstar \textcolor{red}{EKS ~CI/CD ~Pipeline ~using ~Jenkins, ~Docker, ~Helm, ~Kubernetes ~etc.}
$$

1. Launch EC2 :
	* **t2.medium** for **Jenkins**
	* **t2.micro** for **Kubernetes**
	* Amazon Linux *(my case)*
	* Security Group: Inbound Rule= TCP port 22, 8080
2. Install **Jenkins**
	* sudo su -
	* Install OpenJDK 1.8
	* ls -ltra >> .bash_profile >> JAVA_HOME
	* [Jenkins Installation](https://github.com/devops-parth/EKS/blob/master/1_Jenkins_Installation.MD)
	* service jenkins status/start
	* chkconfig jenkins on *#same as enabling*
	* netstat -ntlp *#:::8080 used by Java*
	* Login to Jenkins:8080 and cat the Password
3. Install **Maven** & **GIT**
	*  [Maven and Git Installation](https://github.com/devops-parth/EKS/blob/master/2_Maven_Git.MD)
	* Install Maven >> ls -ltra >> .bash_profile >> $M2
4. Install **Docker**
	* [Docker Installation](https://github.com/devops-parth/EKS/blob/master/3_Docker.MD)
5. Create **EKS Cluster** in  **Kubernetes Server**
	* **t2.micro**
	* Amazon Linux *(my case)*
	* Security Group: Inbound Rule= TCP port 22
	* [EKS Cluster Installation](https://github.com/devops-parth/EKS/blob/master/4_EKS_SetupAWS.MD)
6. Install & Configure **HELM**(Version 3) in **Jenkins Server**
	* Attach the same IAM Role to Jenkins Server that was used in K8s Management Server
	* [HELM Installation](https://github.com/devops-parth/EKS/blob/master/5_Jenkins_HELM.MD)
	* [Stable HELM Charts](https://github.com/devops-parth/EKS/blob/master/6_HELM_Commands.MD)
	> The deployed HELM chart works because we have copied the config file from .kube of Kubernetes and pasted in Jenkins Server. Also we have given IAM role to communicate with EC2 to Jenkins Server
	* [Stable HELM Charts](https://github.com/devops-parth/EKS/blob/master/6_HELM_Commands.MD)
	* [Create Custom Helm Chart for CD Pipeline](https://github.com/devops-parth/EKS/blob/master/7_HELM_CustomChart.MD)
		* [Helm Files & Jenkinsfile](https://github.com/devops-parth/EKS/tree/master/8_HelmFiles_Jenkinsfile)
	> Create Custom Chart and use ///Edit Pending
	
	$$
	\blacklozenge
	\Large \textcolor{blue}{First~PIPELINE:~Continuous~Integration}
	$$
7. Configure MAVEN & JDK in Global Tool Configuration
	* Jenkins >> Global Tool Configuration
		* ADD JDK:
			* Name: JDK8
			* JAVA_HOME: 
					++ Jenkins Server >> $env | grep -i java >> Copy the JAVA_HOME variable content
		*  ADD MAVEN: 
			* Name: MAVEN3
			* Uncheck Install Automatically 
			* MAVEN_HOME:
				++ Jenkins Server >> $env | grep -i maven >> Copy the M2_HOME variable content
8. Configure Docker Credentials in Jenkins
	* Jenkins >> Manage Credentials >> Global Credentials
		* ADD Credentials:
			* UserName with Password

 9. Goto Jenkins: Create New Item >> Pipeline Job >> Pipeline script from SCM >> GIT >> Provide Jenkinsfile Path & Git Link
	 *  [Jenkinsfile, Dockerfile & POM.xml](https://github.com/devops-parth/EKS/tree/master/9_Jenkins%26Maven)

$$
	\blacklozenge
	\Large \textcolor{blue}{Second~PIPELINE:~Continuous~Deployment}
	$$

10. Check the Jenkinsfile & Update the HELM charts according to the need
	* [Helm Files & Jenkinsfile](https://github.com/devops-parth/EKS/tree/master/8_HelmFiles_Jenkinsfile)
	* Goto Jenkins: Create New Item >> Pipeline Job >> Pipeline script from SCM >> GIT >> Provide Jenkinsfile Path & Git Link [Helm Files & Jenkinsfile](https://github.com/devops-parth/EKS/tree/master/8_HelmFiles_Jenkinsfile)
     * Check with:
     ```sh
     helm list #Jenkins Server 
     kubectl get pods #EKS Management Host/Server
     kubectl get svc #Loadbalancer
     kubectl get nodes
     ```
     * Goto AWS EC2 Dashboard:
	     * Copy the public ip of any of the 2 Worker Nodes
	     * Enable the Port 32750 in Security Group (as per NodePort mentioned in the Service.yaml)
	     * Paste in Browser: ipaddress:32750
11. $$\textcolor{red}{Delete ~release ~from ~JENKINS ~SERVER}$$
	* helm uninstall petclinic-app
	>petclinic-app is the release name we mentioned in Helm command

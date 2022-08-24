# Add stable to helm

```sh
helm repo list  #To check if any repo exist & any Error
>> WARNING: K8s configuration file is group-readable. This is insecure. Location: /var/lib/jenkins/.kube/config
>> chmod 600 /var/lib/jenkins/.kube/config

helm repo add stable https://charts.helm.sh/stable
helm repo search stable/chartname #To get all Repos, $ helm repo search stable

helm install repo stable/<chartname> <releasename>
  
helm pull <chartname>
  
helm package <chartname>

helm uninstall RELEASE_NAME
```

## However, we pull first, modify and then deploy the custom HELM Chart
### Example of MySQL with customisatiion
* Goto Jenkins Server
		1. helm search repo stable/mysql
		2. helm pull stable/mysql     **It will download a tar file (.tgz)*
		3. tar -xvzf mysql-1.6.9.tgz
		4. cd mysql/
		5. ls -ltr >> values.yaml, chart.yaml, templates(Contains all main component yaml files)
		6. If we make any changes in any file inside the templates, then update the VERSION inside Chart.yaml
		7. cd ..
		8. helm package mysql
		9. helm install myppdsqldata mysql-1.6.9.tgz
		10. helm list >> to list the chart
* Goto Kubernetes Server
	1. kubectl get pods
	2. kubectl get svc
* Goto Jenkins Server
		1. helm uninstall myppdsqldata 
### Example of NGINX with customisatiion
* Goto Jenkins Server
		1. helm search repo nginx
		2. helm install ppdNginx stable/nginx-ingress
		3. helm list >> to list the chart


### Jenkins server setup with Helm to deploy into Kubernetes cluster
* Attach the same IAM Role (IAM, EC2, VPC, CloudFormation, Adminstrator) to Jenkins Server that was used in K8s Management Server

## Download and Install helm 
```sh
su - jenkins #$id >>> uid Jenkins to validate 

curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

## Test with helm command
```sh
helm version
helm list

>>ERROR: K8s cluster unreacheable,
Goto Kubernetes Management Server
* sudo su -
* ls -lrta
* cd .kube/
* cat config >> Copy the entire content of config
```

## Copy config file from EKS Management host to Jenkins home directory
```sh
mkdir /var/lib/jenkins/.kube  #copy config file under .kube directory with jenkins ownership
chmod 600 /var/lib/jenkins/.kube/config
```

## Test with helm command
```sh
helm version
```

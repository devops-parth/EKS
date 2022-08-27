# Metric server is required for Horizontal Pod Autoscaler

## What is Metric Server?
```sh
It collects metrics like CPU, memory or Disk IO consumption for containers or nodes, from the Summary API, exposed by Kubelet on each node.
```



[Helm Chart Metric Server](https://artifacthub.io/packages/helm/metrics-server/metrics-server)
[K8s GitHub Metric Server](https://github.com/kubernetes-sigs/metrics-server)
[AWS Guide Metric Server](https://docs.aws.amazon.com/eks/latest/userguide/metrics-server.html)

OR
## Clone Metric-server helm chart on K8 Master
1. git clone https://github.com/devops-parth/EKS.git
2. cd [EKS](https://github.com/devops-parth/EKS)/[10_hpa](https://github.com/devops-parth/EKS/tree/master/10_hpa)/[metric-server](https://github.com/devops-parth/EKS/tree/master/10_hpa/metric-server)/[deploy](https://github.com/devops-parth/EKS/tree/master/10_hpa/metric-server/deploy)/**1.8+**/
[Metric Server Files](https://github.com/devops-parth/EKS/tree/master/10_hpa/metric-server/deploy/1.8%2B)
## Create Metric Server
```sh
kubectl create -f .
kubectl top pods
kubectl top nodes
```

# Configure Horizontal Pod Autoscaler

## Modify deployment.yaml with Resource limits
[Deployment Yaml File](https://github.com/devops-parth/EKS/blob/master/8_HelmFiles_Jenkinsfile/helm/ppdDeploy/templates/deployment.yaml)
```sh
  
          resources:
          limits:
            cpu: '1'
            memory: "500Mi"
          requests:
            cpu: '1'
            memory: "500Mi"
  ```
  
## hpa.yaml file in helm chart
[HPA & MEM Yaml File](https://github.com/devops-parth/EKS/blob/master/8_HelmFiles_Jenkinsfile/helm/ppdDeploy/templates/petclinic-mem-hpa.yaml)
 ```sh
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: petclicnic-hpa
spec:
  maxReplicas: 5
  minReplicas: 1
  scaleTargetRef:
    apiVersion: extensions/v1beta1
    kind: Deployment
    name: petclinic-deployment
  targetCPUUtilizationPercentage: 50
  ```
## Install siege package on K8 Master

### Siege is a stress tester for HTTP/HTTPS


### If EKS or AKS Management Host OS is RHEL, then execute below commands
```sh
yum -y install epel-release
yum -y install siege
```
### If EKS Management Host OS is Amazon Linux, then execute below commands
```sh
sudo amazon-linux-extras install epel
yum install siege -y
```
### Send fake load to Application URL 
```sh
siege -q -c 5 -t 2m http://ip:port
q = quiet mode
c = concurrent
2m = time period
```

## Checking at EKS Management HOST
```sh
kubectl top pods
kubectl get hpa
```
> When the stress is released, kubectl get hpa status for CPU will reduce but the PODS will remain for 5 mins for its cooling period and then gets deleted.

# Auto scaling of Pods Based on Memory Utilization
## Modify deployment.yaml with Resource limits
[Deployment Yaml File](https://github.com/devops-parth/EKS/blob/master/8_HelmFiles_Jenkinsfile/helm/ppdDeploy/templates/deployment.yaml)
```sh
resources:
   limits:
     cpu: '1'
     memory: '1G'
   requests:
     cpu: '1'
     memory: '1G'
```
>Increase the memory as it will be manahed by Memory Utilization
 ## memory-hpa.yaml in Application Helm chart
 [HPA & MEM Yaml File](https://github.com/devops-parth/EKS/blob/master/8_HelmFiles_Jenkinsfile/helm/ppdDeploy/templates/petclinic-mem-hpa.yaml)
 ```sh
 apiVersion: autoscaling/v2beta1
kind: HorizontalPodAutoscaler
metadata:
  name: petclinic-memory-hpa
spec:
  maxReplicas: 5
  minReplicas: 1
  scaleTargetRef:
    apiVersion: extensions/v1beta1
    kind: Deployment
    name: petclinic-deployment
  metrics:
  - type: Resource
    resource:
      name: memory
      targetAverageUtilization: 50
 ```
 
 
# Deploy the Job from Jenkins
## Checking at EKS Management HOST
```sh
kubectl top pods
kubectl get pods #COPYtheDeploymentNAME
kubectl exec -it PASTEtheDeploymentNAME bash
>> Inside the POD
```
## Install stress package inside the Application pod
 ```sh
 >> Inside the POD
 apt-get update
 apt-get install stress
 ```
## Execute stress command to increase memory utilization on pod
```sh
 >> Inside the POD
stress --vm 1 --vm-bytes 500M  
#Run in a different terminal & press Ctrl + C to stop the stress
```
* CHECK:
```
kubectl top pods
kubectl get hpa
kubecyl get pods
```





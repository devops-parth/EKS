$$
\Huge\textcolor{purple}{Elasticsearch, ~Fluentd ~and ~Kibana}
$$

## Go to EKS Management Host/Server
1. git clone https://github.com/devops-parth/EKS.git
2. cd /[EKS](https://github.com/devops-parth/EKS)/**11_EFK**/

##Check and Create the Namespace
```sh
kubectl get namespace
kubectl create namespace efklog 
```

##Create the EFK
```sh
kubectl create -f .
kubectl get pods -n efklog
kubectl get svc -n efklog #to get the PORT No.
```
> Elastic Search : ClusterIP because it is internal
>  Kibana : LoadBalancer beacuse it is public facing

## Check Logs
```sh
kubectl get pods -n efklog  # for PODNAME
kubectl logs PODNAME -n efklog
      #kubectl logs PODNAME -n kubesystem
```

-   Goto AWS EC2 Dashboard:
    -   Copy the public ip of any of the 2 Worker Nodes
    -   Enable the Port 31228 in Security Group (as per LoadBalancer mentioned in the (*kubectl get svc -n efklog*))
    -   Paste in Browser: ipaddress:31228

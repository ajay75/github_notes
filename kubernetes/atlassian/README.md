# To install Bamboo on minikube
``` minikube delete ( Clean everything from the local cluster)

 minikube config set memory 8192

 minikube start

 minikube addons enable ingress

 minikube ip

 add the ip address to /etc/hosts
  <ip-adress> bamboo-server.minikube.local

```

## Kubectl command sequence to run the bamboo-server

```
  cd atlassian-bamboo/template

  kubectl config set-context $(kubectl config current-context) --namespace=atlassian-bamboo

  kubectl create -f namespace.yaml

  kubectl create -f pvc.yaml

  kubectl create -f pv.yaml

  kubectl create -f deployment.yaml

  kubectl create -f service.yaml

  kubectl create -f ingress.yaml


```
# # To configure postgresql
```
helm repo add bitnami https://charts.bitnami.com/bitnami

helm install bamboodb --set postgresqlPassword=secretpassword,postgresqlDatabase=bamboo bitnami/postgresql
```
## clusterIP of postgresql to use in the custom bamboo-server installation

```
postgresqlUsername: postgres
password: secretpassword
get svc -o wide to get the <ip-address>

```

## Temporary Atlassian Bamboo license key
```

```
## Change the Bamboo-server Broken client url using the ip address of pod

```
 kubectl get pod <bamboo-server-pod> -o wide, whatever the ip you get replace with localhost
 in this example replacement ip is tcp://172.17.0.4:54663
failover:(tcp://172.17.0.4:54663?wireFormat.maxInactivityDuration=300000)?initialReconnectDelay=15000&maxReconnectAttempts=10

''

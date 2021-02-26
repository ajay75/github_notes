## Create an NGINX Pod
```
kubectl run nginx --image=nginx
```
## Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)
```
kubectl run nginx --image=nginx  --dry-run=client -o yaml
```
## General K8s commands
```
kubectl get nodes
kubectl get pods
kubectl get pods --all-namespaces
kubectl get pods -o wide (provides details which node it's running on)
```

For editing the allready running pod us the impartive command
```
kubectl get pod <anmeofPod> -o yaml > <output>.yaml
```
To create configMap
```
kubectl create configMap my-config-map --from-literal=name=ajay
```
To create Secret available options are docker-registry/generic/tls
So Note the example with generic
```
kubectl create secret generic my-secret --from-literal=DB_Host=sql01 --from-literal=DB_User=root --from-literal=DB_Password=password123
```
To check which user is running the command inside the container Note the space between whoami and ---
```
kubectl exec <running-pod> -- whoami
```

## Create a deployment
```
kubectl create deployment --image=nginx nginx
```
## Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)
```
kubectl create deployment --image=nginx nginx --dry-run -o yaml
```
## Generate Deployment with 3 Replicas
```
kubectl create deployment nginx --image=nginx --replicas=3
```
## scale a nginx deployment to 2
```
kubectl scale deployment nginx --replicas=2
```
## Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379
```
kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml
## OR by the command
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml
```
## Create a Service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes
```
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
```
## To check the k8s resource apiVersion let's say replicaset
```
kubectl explain replicaset | grep VERSION
kubectl explain pods --recursive | grep envFrom
```
## Taint and toleration-- Taint a node 
- Master node is already tainted to check
```
kubectl describe node kubemaster | grep Taint
```
- To remove the taint on the master
```
kubectl taint nodes master/controlplane node-role.kubernetes.io/master:NoSchedule-
```
- Ther are 3 taint effect:
    - NodSchedule (pod will not schedule on Node)
    - PreferNoSchedule ( Try to avoid placing a pod but no gurantee)
    - NoExecute( New pod will not be schedule and existing pod if any will be evicted.)
```
kubectl taint node node-name key=value:taint-effect
kubectl taint node node1 app=blue:NoSchedule
In the Pod.yaml file add toleration under container section
tolerations: WITH QUOTES
 - key: "app"
   operator: "Equal"
   value: "blue"
   effect: "NoSchedule"
```
## Node Affinity
- requiredDuringSchedulingIgnoreDuringExecution
- preferredDuringSchedulingIgnoreDuringExecution
- requiredDuringSchedulingRequiredDuringExecution

## Multicontain pods
```
kubectl run yellow --image=busybox --restart=Never --dry-run=client -o yaml > pod.yaml
- To Check logs
kubectl -n elastic-stack logs app
```
     

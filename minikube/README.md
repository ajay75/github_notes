## minikube tls
```
minikube addons enable ingress
kubectl run nginx --image=nginx
        OR
kubectl create deploy nginx --image=nginx --dry-run -o yaml > deployment.yaml  
kubectl create -f deployment.yaml      
kubclt expose deployment nginx --port 80
```
## create ingress.yaml file 
```
vi ingress.yaml

---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: nginx
spec:ku
 rules:
  - host: nginx.local
    http:
     paths:
      - backend:
         serviceName: nginx
         servicePort: 80
```
curl nginx.local
## create self-signed cert and key using openssl
 - req (request type here x509)
 - newkey since don't have key so it will create new key rsa:4096
 - sha256 algorith sha256
 - nodes (NO DES means we don't want to have password protected key)
 - keyout (name of key it is in this example tls.key)
 - out (out is tls.crt certificate)
 - subj (CN common Name for site in this example nginx.local)
 - days ( how long certifcate should not expire)
 
 ```
 openssl req -x509 -newkey rsa:4096 -sha256 -nodes -keyout tls.key -out tls.crt -subj "/CN=nginx.local" -days 365

```
## Create secret using imparitive command with key and cert as data
```
kubectl create secret tls nginx-local-tls --cert=tls.crt --key=tls.key 

```
## update the ingress with the tls 

```
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: nginx
spec:
 tls:
  - secretName: nginx-local-tls
    hosts:
    - nginx.local
 rules:
  - host: nginx.local
    http:
     paths:
      - backend:
         serviceName: nginx
         servicePort: 80
```
# Apply the changes to cluster
```
kubectl apply -f ingress.yaml ( to update the changes in the cluster)
curl -k https://nginx.local
curl --cacert tls.crt  https://nginx.local

```

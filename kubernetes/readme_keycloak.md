helm install -n keycloak afatds -f values.yaml bitnami/keycloak



NAME: afatds
LAST DEPLOYED: Fri Mar 12 15:14:49 2021
NAMESPACE: keycloak
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
** Please be patient while the chart is being deployed **

Keycloak can be accessed through the following DNS name from within your cluster:

    afatds-keycloak.keycloak.svc.cluster.local (port 80)

To access Keycloak from outside the cluster execute the following commands:

1. Get the Keycloak URL and associate its hostname to your cluster external IP:

   export CLUSTER_IP=$(minikube ip) # On Minikube. Use: `kubectl cluster-info` on others K8s clusters
   echo "Keycloak URL: http://keycloak.local/auth"
   echo "$CLUSTER_IP  keycloak.local" | sudo tee -a /etc/hosts

2. Access Keycloak using the obtained URL.
3. Access the Administration Console using the following credentials:

  echo Username: user  
  echo Password: $(kubectl get secret --namespace keycloak afatds-keycloak-env-vars -o jsonpath="{.data.admin-password}" | base64 --decode)

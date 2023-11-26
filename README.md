# Sublime cluster 
This repository handles the kubernetes cluster of the Sublime microservices.

Tutorials used:

https://medium.com/@TheLe0/running-a-net-application-on-a-local-kubernetes-cluster-1aff3537f755

# Delete pods
```bash
kubectl delete pods --all
```

# Applying kubernetes configuration
```kubectl apply -f {file}```

## Cluster creation
```k3d cluster create sublimeapp --servers 1 --agents 1 --port 9080:80@loadbalancer```
or
```k3d cluster create sublimeapp```
## Pull docker image from dockerhub
```docker pull olmantz/user_api:latest```

## Adding backend to cluster
```k3d image import olmantz/user_api:latest -c sublimeapp```
## Apply manifest
```kubectl apply -f userAPI.yaml```

## Port forwarding for local use
```kubectl port-forward svc/traefik -n kube-system 8080:80```



# Creating K8S token securely
## 1. Generate token and create  secret - K8s_TOKEN
```docker exec k3d-sublimeapp-server-0 cat /var/lib/rancher/k3s/server/node-token```

```kubectl create secret generic ocelot-secrets --from-literal=K8sToken='K8s_TOKEN'```
K8S_token defined in secrets
## 2. Using the secret in the deployment yaml file
Added secret in gateway.yaml file


# Sublime cluster 
This repository handles the kubernetes cluster of the Sublime microservices.

Tutorials used:

https://medium.com/@TheLe0/running-a-net-application-on-a-local-kubernetes-cluster-1aff3537f755
Using Task-Go and local .kube/config.
# Delete pods
```bash
kubectl delete pods --all
```
# Accessing Azure cluster
az aks get-credentials --resource-group WebApp --name Cluster
# Applying kubernetes configuration 
```kubectl apply -f {file}```

## Cluster creation
### Option 1 - Using Task
As defined in the Taskfile.yaml
```task create-cluster```
```task set-kubeconfig```
```task update-gitignore```
```task apply-yamls```

After waiting for a bit, Traefik pod is slow:

```task port-forward```

Or  all chained as:

```task create-cluster update-gitignore apply-yamls port-forward```

### Option 2 - Manually setting cluster with local config
With local kube configuration file 
```k3d cluster create sublimeapp --servers 1 --agents 1 --port 8080:80@loadbalancer```
creating local configuration
```k3d kubeconfig get sublimeapp > sublimeapp-kubeconfig.yaml```
and setting environmental variable
```$env:KUBECONFIG = (Get-Location).Path + "\sublimeapp-kubeconfig.yaml"```


or

### Option 2 - Simple cluster
```k3d cluster create sublimeapp```
## Pull docker image from dockerhub
Not necessary```docker pull olmantz/user_api:latest```

## Adding backend to cluster
Not necessary ```k3d image import olmantz/user_api:latest -c sublimeapp```
## Apply manifest
```kubectl apply -f userAPI.yaml```

## Port forwarding for local use
```kubectl port-forward svc/traefik -n kube-system 8080:80```



# Creating K8S token securely
For Ocelot
## 1. Generate token and create  secret - K8s_TOKEN
```docker exec k3d-sublimeapp-server-0 cat /var/lib/rancher/k3s/server/node-token```

```kubectl create secret generic ocelot-secrets --from-literal=K8sToken='K8s_TOKEN'```

K8S_token defined in secrets
## 2. Using the secret in the deployment yaml file
Added secret in gateway.yaml file


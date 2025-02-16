# Kubernetes (K8s) Commands Cheat Sheet

This document provides an overview of commonly used Kubernetes (K8s) commands along with their arguments and example usage.

---
## 1. Cluster Information
### Get cluster information
```sh
kubectl cluster-info
```

### Get all nodes in the cluster
```sh
kubectl get nodes
```

---
## 2. Working with Namespaces
### List all namespaces
```sh
kubectl get namespaces
```

### Create a new namespace
```sh
kubectl create namespace my-namespace
```

### Delete a namespace
```sh
kubectl delete namespace my-namespace
```

---
## 3. Managing Pods
### List all pods in the current namespace
```sh
kubectl get pods
```

### Get pods in a specific namespace
```sh
kubectl get pods -n my-namespace
```

### Describe a pod
```sh
kubectl describe pod my-pod
```

### Delete a pod
```sh
kubectl delete pod my-pod
```

### Apply a YAML configuration file
```sh
kubectl apply -f pod.yaml
```

### Run a new pod interactively
```sh
kubectl run my-pod --image=nginx --restart=Never
```

### Show YAML of pod creation without create a new pod 
```sh
kubectl run redis --image=redis --dry-run=client -o yaml
```

---
## 4. Managing Deployments
### List all deployments
```sh
kubectl get deployments
```

### Describe a deployment
```sh
kubectl describe deployment my-deployment
```

### Scale a deployment
```sh
kubectl scale deployment my-deployment --replicas=3
```

### Update a deployment
```sh
kubectl set image deployment/my-deployment nginx=nginx:latest
```

### Roll back to a previous deployment revision
```sh
kubectl rollout undo deployment my-deployment
```

---
## 5. Managing Services
### List all services
```sh
kubectl get services
```

### Describe a service
```sh
kubectl describe service my-service
```

### Expose a deployment as a service
```sh
kubectl expose deployment my-deployment --type=LoadBalancer --port=80
```

---
## 6. Managing ConfigMaps and Secrets
### Create a ConfigMap
```sh
kubectl create configmap my-config --from-literal=key=value
```

### Describe a ConfigMap
```sh
kubectl describe configmap my-config
```

### Create a Secret
```sh
kubectl create secret generic my-secret --from-literal=password=my-password
```

### Describe a Secret
```sh
kubectl describe secret my-secret
```

---
## 7. Logs and Debugging
### View pod logs
```sh
kubectl logs my-pod
```

### Get interactive shell access to a pod
```sh
kubectl exec -it my-pod -- /bin/sh
```

### Get events in a namespace
```sh
kubectl get events -n my-namespace
```

---
## 8. Resource Management
### Get resource usage of nodes
```sh
kubectl top nodes
```

### Get resource usage of pods
```sh
kubectl top pods
```

### Delete all pods in a namespace
```sh
kubectl delete pods --all -n my-namespace
```

### Kubernetes Objects Reference: 



| Object     | Kind       | API Version | Description |
|------------|-----------|-------------|-------------|
| Cluster    | -         | -           | Represents the Kubernetes cluster itself |
| Node       | Node      | v1          | A worker machine in Kubernetes, part of the cluster |
| Namespace  | Namespace | v1          | A logical partition within the cluster to group resources |
| Pod        | Pod       | v1          | The smallest deployable unit in Kubernetes, contains containers |
| Deployment | Deployment | apps/v1     | Manages the deployment of pods, ensuring desired state |
| Service    | Service   | v1          | Exposes and balances network traffic to pods |
| ConfigMap  | ConfigMap | v1          | Stores non-confidential configuration data for pods |
| Secret     | Secret    | v1          | Stores confidential data such as passwords and tokens |
| Event      | Event     | v1          | Records occurrences and changes in the cluster |

---
## Conclusion
This document provides a quick reference for essential Kubernetes commands. For more details, refer to the official [Kubernetes documentation](https://kubernetes.io/docs/).


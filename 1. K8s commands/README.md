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
| Job | Job | batch/v1 | Creates one or more pods and ensures that a specified number of them successfully terminate | 
| CronJob | CronJob | batch/v1 | Similar to Job, but runs on a scheduled time, like cron jobs in Unix |
| DaemonSet | DaemonSet | apps/v1 | Ensures that all (or some) nodes run a copy of a pod. Ideal for logging or monitoring agents | 
| StatefulSet | StatefulSet | apps/v1 | Manages the deployment of stateful applications, maintaining a sticky identity for each pod | 
| Deployment | Deployment | apps/v1     | Manages the deployment of pods, ensuring desired state |
| ReplicationSet | ReplicationSet | apps/v1     | Ensures a specified number of pod replicas are running. Supports set-based label selectors. Typically used in Deployments |
| ReplicationController (Legacy object) | ReplicationController | v1     | Ensures a specified number of pod replicas are running. Uses equality-based label selectors. Legacy object mostly replaced by ReplicationSet in modern Kubernetes setups |
| Service    | Service   | v1          | Exposes and balances network traffic to pods |
| Ingress | Ingress | networking.k8s.io/v1 | Manages external access to services in a cluster, typically HTTP. Allows defining rules to route traffic |
| ConfigMap  | ConfigMap | v1          | Stores non-confidential configuration data for pods |
| Secret     | Secret    | v1          | Stores confidential data such as passwords and tokens |
| Event      | Event     | v1          | Records occurrences and changes in the cluster |
| Role | Role | rbac.authorization.k8s.io/v1 | Defines a set of permissions within a specific namespace. It is used to control access to resources within that namespace, typically combined with `RoleBindings` to assign the role to users or service accounts |
| RoleBinding | RoleBinding | rbac.authorization.k8s.io/v1 | Binds a Role to a user, group, or service account within a specific namespace. It grants the permissions defined in the Role to the referenced entities, allowing them to access the specified resources |
| ClusterRole | ClusterRole | rbac.authorization.k8s.io/v1 | Defines a set of permissions across all namespaces in a cluster |
| ClusterRoleBinding | ClusterRoleBinding | rbac.authorization.k8s.io/v1 | Binds a ClusterRole to users, groups, or service accounts across the whole cluster |
| PersistentVolume | PersistentVolume | v1 | Represents a piece of storage in the cluster that has been provisioned by an administrator |
| PersistentVolumeClaim | PersistentVolumeClaim | v1 | A request for storage by a user, binding to a PersistentVolume |
| ServiceAccount | ServiceAccount | v1 | Represents an identity for processes that run in a pod. It’s associated with a set of credentials and can be used for accessing the Kubernetes API |
| NetworkPolicy | NetworkPolicy | networking.k8s.io/v1 | Defines the rules for controlling network access between pods in a Kubernetes cluster. |

This is a general overview of some of the key Kubernetes objects. There are others, but these are some of the most commonly used ones. Let me know if you'd like further details on any specific object!

---
## Conclusion
This document provides a quick reference for essential Kubernetes commands. For more details, refer to the official [Kubernetes documentation](https://kubernetes.io/docs/).


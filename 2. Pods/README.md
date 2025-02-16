# Kubernetes Pod: 

This repository contains a Kubernetes YAML definition for deploying a Spring Boot application along with an Application Performance Monitoring (APM) agent (Elastic APM in this case). The setup includes an init container to perform any necessary initialization tasks before the main containers start.

---
## 1. Pod creation: 
### Create new pod using `nginx` image. 
```sh
kubectl apply -f webapp.yaml
```

### Create new pod using `nginx` image with two container. 
```sh
kubectl apply -f webapp-v2.yaml
```

### Create a Kubernetes pod that runs a Spring Boot application and sets environment variables:
```sh
kubectl apply -f springboot-pod.yaml
```

### Create a Kubernetes pod that runs a Spring Boot application and APM Agent:
This command can run a pod using Kubernetes YAML definition for deploying a Spring Boot application along with an Application Performance Monitoring (APM) agent (Elastic APM in this case).

```sh
kubectl apply -f springboot-pod-with-apm.yaml
```

---
## 2. Edit a Running Pod:

If the pod is already running and you want to make a change to the container (such as modifying environment variables, resource limits, etc.), you can do this directly by editing the pod's configuration:

```sh
kubectl edit pod <pod-name>
```
This will open the pod configuration in the default text editor. You can then modify the container settings (such as environment variables, image, etc.). Once you save and close the editor, Kubernetes will apply the changes.

## 3. Deleting Pods:

If you can't modify the pod directly (like if it's not part of a deployment), you can delete the existing pod and recreate it using the updated configuration:

```sh
kubectl delete pod <pod-name>
```

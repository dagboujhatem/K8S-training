# Kubernetes Deployment Management

This document provides a guide on creating and managing deployments in Kubernetes. A **deployment** in Kubernetes is an abstraction that manages a set of replicas of your application, ensuring that the desired number of replicas is running at any given time. Deployments handle scaling, updates, and rollbacks for your application pods.

## Table of Contents
- [What is a Kubernetes Deployment?](#what-is-a-kubernetes-deployment)
- [Creating a Deployment](#creating-a-deployment)
- [Scaling a Deployment](#scaling-a-deployment)
- [Updating a Deployment](#updating-a-deployment)
- [Rolling Back a Deployment](#rolling-back-a-deployment)
- [Listing Deployments](#listing-deployments)
- [Deleting a Deployment](#deleting-a-deployment)
- [Best Practices for Managing Deployments](#best-practices-for-managing-deployments)

---

## What is a Kubernetes Deployment?
A Kubernetes **deployment** is used to manage the deployment and scaling of applications in the cluster. It ensures that a specified number of pod replicas are running at any given time and provides declarative updates to applications. Kubernetes deployments also support rolling updates, which allow you to deploy new versions of your application without downtime.

Some key features of a deployment:
- **Scaling**: Easily scale up or down the number of pod replicas.
- **Rolling Updates**: Update your application with zero downtime.
- **Rollbacks**: Easily roll back to a previous stable version of your application.

---

## Creating a Deployment

To create a deployment in Kubernetes, you can define it using a YAML file. Below is an example of how to define a simple deployment for a web application.

### Example: Creating a Deployment

**deployment.yaml**:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: my-image:latest
          ports:
            - containerPort: 8080
``` 

## Scaling a Deployment:
To scale a deployment and increase or decrease the number of pod replicas, use the following command:
```
kubectl scale deployment my-deployment --replicas=5
``` 
This command scales the my-deployment deployment to 5 replicas.

To verify the scaling, run:
```
kubectl get deployments
``` 
This will display the current number of replicas for each deployment.
## Updating a Deployment:
To update a deployment (for example, to use a new container image), you can modify the deployment definition and re-apply it:
**deployment.yaml**:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: my-new-image:latest  # Updated image
          ports:
            - containerPort: 8080
``` 

Apply the updated deployment:
```
kubectl apply -f deployment.yaml
``` 
Kubernetes will perform a rolling update, gradually replacing the old pods with new ones that use the updated image.
## Rolling Back a Deployment:
If there’s an issue with a deployment update and you need to roll back to a previous version, you can do so with the following command:
```
kubectl rollout undo deployment/my-deployment
``` 

This will roll back the deployment to the previous revision. You can check the rollout history with:
```
kubectl rollout history deployment/my-deployment
``` 
To see more details about a specific revision, run:
```
kubectl rollout history deployment/my-deployment --revision=<revision-number>
``` 
## Listing Deployments:
To list all deployments in your Kubernetes cluster, use the following command:

```
kubectl get history deployments
``` 
This will show you the list of deployments, the number of pods in each deployment, and other related information.

## Deleting a Deployment:
To delete a deployment, use the following command:
```
kubectl delete deployment my-deployment
``` 
This will remove the deployment and all associated pods from the cluster.

## Best Practices for Managing Deployments:
- Version Control: Always use versioned container images for your deployments. Avoid using the latest tag to prevent confusion.

- Rolling Updates: Use rolling updates for zero downtime during application updates. Ensure that your application can handle rolling updates without issues.

- Resource Requests and Limits: Define resource requests and limits for your containers to ensure that your application performs well in a multi-tenant cluster.

    **Example :**
    ```yaml
    resources:
    requests:
        cpu: "500m"
        memory: "512Mi"
    limits:
        cpu: "1"
        memory: "1Gi"
    ``` 
- Health Checks: Always define readiness and liveness probes to ensure your pods are healthy and ready to serve traffic.

    **Example :**
    ```yaml
    readinessProbe:
    httpGet:
        path: /healthz
        port: 8080
    initialDelaySeconds: 5
    periodSeconds: 10

    livenessProbe:
    httpGet:
        path: /healthz
        port: 8080
    initialDelaySeconds: 15
    periodSeconds: 20
    ``` 
- Namespace Usage: Create deployments within specific namespaces to logically group your resources and isolate them.
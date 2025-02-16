# Kubernetes ReplicationController & ReplicaSet

## Overview
In Kubernetes, **ReplicationController** and **ReplicaSet** are used to ensure the availability and scalability of pods. They maintain a specified number of pod replicas running at all times.

---

## ReplicationController
A **ReplicationController** is responsible for ensuring that a specified number of pod replicas are running. If a pod fails, the controller creates a new one to replace it.

### Creating a ReplicationController
Example YAML manifest:
```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: my-replication-controller
spec:
  replicas: 3
  selector:
    app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: nginx
          ports:
            - containerPort: 80
```

Apply the manifest:
```sh
kubectl apply -f replication-controller.yaml
```

### Checking Status
```sh
kubectl get replicationcontroller
```

### Deleting a ReplicationController
```sh
kubectl delete replicationcontroller my-replication-controller
```

---

## ReplicaSet
A **ReplicaSet** is the successor of ReplicationController and provides the same functionality but with more powerful selectors.

### Creating a ReplicaSet
Example YAML manifest:
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
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
          image: nginx
          ports:
            - containerPort: 80
```

Apply the manifest:
```sh
kubectl apply -f replicaset.yaml
```

### Checking Status
```sh
kubectl get replicaset
```

### Scaling a ReplicaSet
```sh
kubectl scale --replicas=5 replicaset my-replicaset
```

### Deleting a ReplicaSet
```sh
kubectl delete replicaset my-replicaset
```

---

## Differences Between ReplicationController and ReplicaSet
| Feature            | ReplicationController | ReplicaSet |
|--------------------|----------------------|-----------|
| API Version       | v1                   | apps/v1   |
| Selector Support  | Basic selectors      | Label selectors with match expressions |
| Recommended?      | No (deprecated)      | Yes       |

---

## Best Practices
- **Use ReplicaSet instead of ReplicationController**, as it provides better selector capabilities.
- **Use Deployments instead of ReplicaSets** for better lifecycle management.
- **Ensure proper resource limits** to avoid overloading nodes.
- **Monitor replica counts** to ensure high availability.

---

## Conclusion
ReplicationController and ReplicaSet are essential for managing pod availability in Kubernetes. While ReplicationController is now mostly replaced by ReplicaSet, using Deployments is often the best choice for real-world applications.


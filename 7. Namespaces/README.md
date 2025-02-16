# Kubernetes Namespace Management

This document provides a guide on managing namespaces in Kubernetes. Namespaces in Kubernetes are used to organize and manage resources in a cluster. Each namespace provides a scope for names, helping to avoid name collisions and enable resource isolation within the cluster.

## Table of Contents
- [What is a Kubernetes Namespace?](#what-is-a-kubernetes-namespace)
- [Creating a Namespace](#creating-a-namespace)
- [Listing Namespaces](#listing-namespaces)
- [Deleting a Namespace](#deleting-a-namespace)
- [Using Namespaces in Resources](#using-namespaces-in-resources)
- [Best Practices for Managing Namespaces](#best-practices-for-managing-namespaces)

---

## What is a Kubernetes Namespace?

Namespaces in Kubernetes provide a mechanism for isolating resources in a cluster. Each resource, such as pods, services, and deployments, can be assigned to a specific namespace. This allows for the following benefits:
- **Isolation**: Prevents resources in different namespaces from interfering with each other.
- **Resource Management**: Enables resource quotas, limits, and policies specific to each namespace.
- **Organization**: Helps organize resources logically, especially in larger clusters.

Kubernetes namespaces are typically used for different environments (e.g., development, staging, production) or different teams working within the same cluster.

---

## Creating a Namespace

To create a namespace in Kubernetes, you can use the `kubectl` command-line tool. The following example shows how to create a new namespace called `dev`:

```bash
kubectl create namespace dev
```


Alternatively, you can create a namespace using a YAML definition file:

```bash
kubectl apply -f namespace-dev.yaml
```

## Listing Namespaces

To delete a namespace and all its resources, use the following command:

```bash
kubectl get namespaces
```
This will display a list of all namespaces along with their statuses.

## Deleting a Namespace

To delete a namespace and all its resources, use the following command:

```bash
kubectl delete namespace <namespace-name>
```

For example, to delete the dev namespace:

```bash
kubectl delete namespace dev
```

Be cautious when deleting namespaces, as it will remove all resources within the namespace.

## Using Namespaces in Resources
When creating resources (such as pods, services, or deployments), you can specify the namespace under which they should be created. For example:


```bash
kubectl apply -f pod-definition.yaml
```

Alternatively, you can also use the --namespace flag when running kubectl commands. For example: 


```bash
kubectl get pods --namespace=dev
```

## Best Practices for Managing Namespaces: 

- Separation of Environments: Use separate namespaces for different environments (e.g., dev, staging, prod) to avoid accidental conflicts.
- Namespace for Teams: Assign different namespaces for different teams or projects. - This provides better isolation and simplifies resource management.
- Resource Quotas: Use resource quotas to limit the usage of resources within a namespace, preventing one namespace from consuming all cluster resources.

```bash
kubectl create quota <quota-name> --namespace=<namespace> --cpu=<cpu-limit> --memory=<memory-limit>
```

- Role-Based Access Control (RBAC): Use RBAC to manage user access to specific namespaces. This ensures that users only have access to resources in the namespaces they are authorized to use.


```bash
kubectl apply -f dev-role-binding.yaml
```
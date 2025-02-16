# Kubernetes Roles, RBAC & User Management

## Overview
Kubernetes uses **Role-Based Access Control (RBAC)** to regulate permissions and access to resources within a cluster. **Roles** define what actions are permitted within a specific namespace, and **Users** are assigned roles via RoleBindings or ClusterRoleBindings.

---

## Roles in Kubernetes
A **Role** grants permissions within a specific namespace. It defines a set of rules specifying which resources can be accessed and what actions can be performed on them.

### Creating a Role
Example YAML manifest:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""]  # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```
Apply the manifest:
```sh
kubectl apply -f role.yaml
```

### Checking Existing Roles
```sh
kubectl get roles --namespace=default
```

### Viewing Role Details
```sh
kubectl describe role pod-reader --namespace=default
```

---

## RoleBinding
A **RoleBinding** associates a Role with a user, group, or service account, granting them the defined permissions.

### Creating a RoleBinding
Example YAML manifest:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-reader-binding
  namespace: default
subjects:
- kind: User
  name: jane  # User that will be granted the role
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```
Apply the manifest:
```sh
kubectl apply -f rolebinding.yaml
```

### Checking RoleBindings
```sh
kubectl get rolebindings --namespace=default
```

---

## ClusterRole
A **ClusterRole** is similar to a Role but applies at the cluster level rather than a specific namespace.

### Creating a ClusterRole
Example YAML manifest:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-admin-reader
rules:
- apiGroups: [""]
  resources: ["nodes", "pods"]
  verbs: ["get", "list", "watch"]
```
Apply the manifest:
```sh
kubectl apply -f clusterrole.yaml
```

### Checking ClusterRoles
```sh
kubectl get clusterroles
```

---

## ClusterRoleBinding
A **ClusterRoleBinding** grants a ClusterRole to a user, group, or service account across the entire cluster.

### Creating a ClusterRoleBinding
Example YAML manifest:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-admin-binding
subjects:
- kind: User
  name: admin-user  # User that will be granted the role
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: cluster-admin-reader
  apiGroup: rbac.authorization.k8s.io
```
Apply the manifest:
```sh
kubectl apply -f clusterrolebinding.yaml
```

### Checking ClusterRoleBindings
```sh
kubectl get clusterrolebindings
```

---

## Managing Users in Kubernetes
Kubernetes does not manage users directly; authentication is handled by external identity providers, certificates, or authentication plugins. Here are some common methods for managing users:

### Creating a User with a Client Certificate
1. Generate a private key:
   ```sh
   openssl genrsa -out user-key.pem 2048
   ```
2. Create a Certificate Signing Request (CSR):
   ```sh
   openssl req -new -key user-key.pem -out user.csr -subj "/CN=jane"
   ```
3. Sign the certificate using the Kubernetes CA:
   ```sh
   openssl x509 -req -in user.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out user-cert.pem -days 365
   ```
4. Add the user to kubeconfig:
   ```sh
   kubectl config set-credentials jane --client-certificate=user-cert.pem --client-key=user-key.pem
   ```

### Assigning a Role to a User
To grant access, use a **RoleBinding** or **ClusterRoleBinding** as described earlier.

### Checking User Permissions
```sh
kubectl auth can-i list pods --as=jane
```

---

## Best Practices
- **Follow the principle of least privilege**: Grant only the permissions necessary for a role.
- **Use RoleBindings for namespace-specific access** and ClusterRoleBindings for cluster-wide access.
- **Regularly audit RBAC settings** to ensure security.
- **Use service accounts for applications** instead of granting access to individual users.
- **Leverage identity providers** like OpenID Connect, LDAP, or Active Directory for authentication.

---

## Conclusion
Roles and RBAC are essential for securing a Kubernetes cluster by controlling who has access to which resources. Managing users properly ensures secure access control and prevents unauthorized operations.


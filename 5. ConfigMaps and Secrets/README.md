# Kubernetes ConfigMaps & Secrets

## Overview
Kubernetes provides **ConfigMaps** and **Secrets** to manage application configuration and sensitive data. These resources allow separating configuration from application code, making deployments more flexible and secure.

---

## ConfigMaps
A **ConfigMap** is a key-value store used to store non-sensitive configuration data such as environment variables, command-line arguments, or configuration files.

### Creating a ConfigMap
1. **From a file:**
   ```sh
   kubectl create configmap my-config --from-file=config.properties
   ```
2. **From a key-value pair:**
   ```sh
   kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2
   ```
3. **Using a YAML manifest:**
   ```yaml
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: my-config
   data:
     key1: value1
     key2: value2
   ```
   Apply the manifest with:
   ```sh
   kubectl apply -f configmap.yaml
   ```

### Using ConfigMaps in Pods
#### As environment variables:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
    - name: example-container
      image: nginx
      env:
        - name: CONFIG_VALUE
          valueFrom:
            configMapKeyRef:
              name: my-config
              key: key1
```

#### As a mounted volume:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
    - name: example-container
      image: nginx
      volumeMounts:
        - name: config-volume
          mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: my-config
```

---

## Secrets
A **Secret** is used to store sensitive information such as passwords, API keys, and TLS certificates. Unlike ConfigMaps, Secrets are base64-encoded.

### Creating a Secret
1. **From a file:**
   ```sh
   kubectl create secret generic my-secret --from-file=secret.properties
   ```
2. **From a key-value pair:**
   ```sh
   kubectl create secret generic my-secret --from-literal=username=admin --from-literal=password=securepassword
   ```
3. **Using a YAML manifest:**
   ```yaml
   apiVersion: v1
   kind: Secret
   metadata:
     name: my-secret
   type: Opaque
   data:
     username: YWRtaW4=  # base64-encoded "admin"
     password: c2VjdXJlcGFzc3dvcmQ=  # base64-encoded "securepassword"
   ```
   Apply the manifest with:
   ```sh
   kubectl apply -f secret.yaml
   ```

### Using Secrets in Pods
#### As environment variables:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
    - name: example-container
      image: nginx
      env:
        - name: SECRET_USER
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: username
```

#### As a mounted volume:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
    - name: example-container
      image: nginx
      volumeMounts:
        - name: secret-volume
          mountPath: /etc/secret
  volumes:
    - name: secret-volume
      secret:
        secretName: my-secret
```

---

## Best Practices
- **Use Secrets for sensitive data**: Do not store sensitive credentials in ConfigMaps.
- **Use RBAC (Role-Based Access Control)**: Restrict access to Secrets.
- **Enable Encryption at Rest**: Use Kubernetes secret encryption to protect data.
- **Use external secret management tools**: Tools like HashiCorp Vault or AWS Secrets Manager can enhance security.

---

## Conclusion
ConfigMaps and Secrets are essential Kubernetes resources that help manage configuration and sensitive data separately from application code. By following best practices, you can ensure security and maintainability in your Kubernetes environment.


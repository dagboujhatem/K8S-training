# Kubernetes Training Advanced Topics Overview
For advanced learners, this section explores deeper topics related to Kubernetes.
## Additional Kubernetes Objects

| Object                | Kind                | API Version                          | Description                                                                 |
|-----------------------|---------------------|--------------------------------------|-----------------------------------------------------------------------------|
| **CustomResourceDefinition (CRD)** | `CustomResourceDefinition` | `apiextensions.k8s.io/v1` | Allows you to define your own custom resources in Kubernetes. |
| **HorizontalPodAutoscaler (HPA)**  | `HorizontalPodAutoscaler` | `autoscaling/v2` | Automatically scales the number of pods based on CPU or other metrics. |
| **VerticalPodAutoscaler (VPA)**    | `VerticalPodAutoscaler`   | `autoscaling.k8s.io/v1` | Automatically adjusts CPU and memory requests/limits based on usage. |
| **PodDisruptionBudget (PDB)**      | `PodDisruptionBudget`      | `policy/v1` | Ensures that a minimum number of pods remain available during voluntary disruptions. |
| **EndpointSlice**      | `EndpointSlice`      | `discovery.k8s.io/v1`               | Provides scalable service endpoint tracking. |
| **LimitRange**         | `LimitRange`         | `v1`                                 | Defines limits for container resources within a namespace. |
| **ResourceQuota**      | `ResourceQuota`      | `v1`                                 | Sets usage limits for resources like CPU, memory, or pods within a namespace. |
| **PodSecurityPolicy** (deprecated) | `PodSecurityPolicy` | `policy/v1beta1` | Defines security settings for pod specifications. |
| **AdmissionControl (MutatingWebhookConfiguration, ValidatingWebhookConfiguration)** | `MutatingWebhookConfiguration`, `ValidatingWebhookConfiguration` | `admissionregistration.k8s.io/v1` | Custom logic for validating or mutating requests to the Kubernetes API. |
| **API Aggregation Layer** | `API aggregation layer` | Varies | Allows you to extend the Kubernetes API with custom resources. |

## Kubernetes Security: 
Security is critical in Kubernetes, and it requires attention to detail at every level.

**Topics:**
- RBAC (Role-Based Access Control)
- Service accounts and authentication
- Pod Security Policies (PSP)
- Network Policies for security
- Secret management

## Scaling and Monitoring: 
This section focuses on scaling applications and monitoring Kubernetes clusters to ensure that they are running smoothly.

**Topics:**
- Horizontal Pod Autoscaling (HPA)
- Vertical Pod Autoscaling (VPA)
- Resource requests and limits
- Prometheus and Grafana for monitoring
- Logging with ELK Stack (Elasticsearch, Logstash, Kibana)

## Deploying Applications: 
Kubernetes simplifies application deployment and management. In this section, we will look at how to deploy applications effectively.

**Topics:**
- Writing Kubernetes manifests
- Rolling updates and Rollbacks
- Continuous Integration and Continuous Deployment (CI/CD) with Kubernetes
- Helm charts for managing deployments

## Advanced Topics: 
For advanced learners, this section explores deeper topics related to Kubernetes. 
**Topics:**
- Custom Resources and Custom Controllers
- Operators and CRDs (Custom Resource Definitions)
- Kubernetes Federation
- Kubernetes and Istio for service mesh
- Managing Kubernetes at scale (Large clusters)

## Basic Deployement in K8s: 
Deploying microservices in Kubernetes involves a few key steps to ensure that each microservice is running properly and can communicate with other services. Here’s a general guide on how to deploy microservices in Kubernetes:
1. **Prepare the Microservices:** Each microservice should be packaged as a Docker container and pushed to a container registry (e.g., Docker Hub, Google Container Registry, AWS ECR).
2. **Define Kubernetes Manifests for Each Microservice:**
You need to define the necessary Kubernetes objects (such as Pods, Deployments, Services, and ConfigMaps) to deploy and manage your microservices.
3. **Set Up Communication Between Microservices:**
Use Kubernetes Services to enable communication between different microservices. A ClusterIP service is ideal for internal communication, which exposes the service only inside the cluster. 

    If the microservices need to communicate with each other, they can use the DNS name of the service (e.g., user-service.default.svc.cluster.local).
4. **Configure Ingress (Optional):**
If you want to expose your microservices to external users, use Ingress to route external HTTP(S) traffic to the correct services.

5. **Apply Kubernetes Manifests:** Once you have the YAML files ready, apply them to your Kubernetes cluster:

## Advanced Deployement in K8s:

### Use Helm (Optional): 
To simplify deployment, you can use Helm, a Kubernetes package manager. Helm allows you to package and deploy your microservices more easily.

You can create a Helm chart for your microservices, and deploy it using:
```sh
helm install user-service ./user-service-chart
```
### Stateful Services in Kubernetes: 
Stateful services are microservices that require persistent storage and need to maintain their state across restarts or failures. Kubernetes provides tools to manage stateful applications using StatefulSets and Persistent Volumes (PV).

Key Concepts for Stateful Services:
- StatefulSet: A Kubernetes object used for managing stateful applications. Unlike Deployments, StatefulSets maintain the identity of pods across restarts and scale operations, ensuring the consistency of state across replicas.
- Persistent Volume (PV): A piece of storage in the Kubernetes cluster that is independent of pods. It allows data to persist even when pods are restarted or rescheduled.
- Persistent Volume Claim (PVC): A request for storage made by a pod. A PVC binds to a PV, which provides the necessary storage.

Example:  StatefulSet for a Stateful Service (e.g., Database):
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: database
spec:
  serviceName: "database"
  replicas: 3
  selector:
    matchLabels:
      app: database
  template:
    metadata:
      labels:
        app: database
    spec:
      containers:
        - name: database
          image: mysql:5.7
          volumeMounts:
            - name: data
              mountPath: /var/lib/mysql
  volumeClaimTemplates:
    - metadata:
        name: data
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 1Gi
```

- ServiceName: The serviceName is used to create DNS entries for each pod, ensuring that each replica of the database can be accessed individually.
- VolumeClaimTemplates: This allows each pod in the StatefulSet to have its own unique PVC, which is tied to a Persistent Volume.
### Distributed Tracing in Kubernetes: 
Distributed tracing is used to track requests across multiple microservices and monitor their performance. This is important in microservice architectures where a request may span multiple services. 

Key Components for Distributed Tracing:
- Instrumentation: Each microservice must be instrumented with tracing libraries to capture data.
- Tracer: The tracer collects data about the lifecycle of a request as it travels through the services.
- Tracing Backend: A centralized system (e.g., `Jaeger`, `Zipkin`) that stores and visualizes the trace data.
### Service Meshes in Kubernetes: 

A service mesh is a dedicated infrastructure layer for managing microservices communication. It abstracts away the complexity of managing communication between microservices, offering features like load balancing, service discovery, security, and observability.

Key Features of Service Meshes:
- Service Discovery: Automatically discovers services in the mesh and manages their endpoints.
- Load Balancing: Distributes requests across service replicas for higher availability and reliability.
- Traffic Routing: Manages traffic between services with advanced routing rules (e.g., blue-green deployments).
- Resiliency: Features like retries, circuit breaking, and timeouts help ensure high availability and fault tolerance.
- Security: Enforces mutual TLS (mTLS) encryption for secure communication between services.
- Observability: Provides metrics, logs, and traces for monitoring services’ health and performance.

**Popular Service Mesh Solutions:** 

- Istio: One of the most popular service mesh solutions, providing advanced traffic management, security, and observability features.
- Linkerd: A lighter, simpler service mesh solution that focuses on ease of use and performance.
- Consul: A service mesh that focuses on service discovery and configuration management.


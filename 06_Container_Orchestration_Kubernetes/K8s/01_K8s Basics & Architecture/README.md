
## **1. What is Kubernetes?**

Kubernetes (K8s) is an **open-source container orchestration platform** that automates the deployment, scaling, and management of containerized applications.

**Why use Kubernetes?**

* Automatically deploys containers across multiple servers (nodes).
* Self-healing: Restarts failed containers, replaces unhealthy ones.
* Horizontal scaling: Adds or removes containers based on demand.
* Service discovery & load balancing.

**Example:**
> You have a web application running in Docker containers. Instead of manually managing 10 containers across 3 servers, K8s handles deployment, scaling, and health checks automatically.

---

## **2. Kubernetes Architecture**

K8s has a **master-worker architecture**.

### **A. Components**

### **1. Master Node (Control Plane)**

Manages the cluster.

| Component                       | Function                                                                    |
| ------------------------------- | --------------------------------------------------------------------------- |
| **API Server (kube-apiserver)** | Entry point for all API requests (kubectl or internal services).            |
| **etcd**                        | Key-value store for cluster state & configuration.                          |
| **Controller Manager**          | Ensures cluster desired state matches actual state (replicas, nodes, etc.). |
| **Scheduler**                   | Assigns workloads (Pods) to worker nodes based on resources and policies.   |

### **2. Worker Node**

Runs the application workloads.

| Component             | Function                                                       |
| --------------------- | -------------------------------------------------------------- |
| **kubelet**           | Agent that ensures containers in Pods are running as expected. |
| **kube-proxy**        | Manages networking and load balancing for services.            |
| **Container Runtime** | Runs containers (Docker, containerd, CRI-O).                   |

### **Pod**

* Smallest deployable unit in Kubernetes.
* Can contain **one or more containers** that share storage/network.

### **Service**

* Exposes pods to the outside world or internally.
* Provides **load balancing**.

### **Other Key Objects**

* **Deployment**: Defines desired state of pods, updates, rollbacks.
* **ConfigMap & Secret**: Manage configuration and sensitive data.
* **PersistentVolume (PV) & PersistentVolumeClaim (PVC)**: Storage management.

---

## **3. Kubernetes Architecture Diagram**

Here’s a simplified diagram:

```
                               +---------------------+
                               |    Master Node      |
                               |---------------------|
                               |  API Server         |
                               |  Scheduler          |
                               |  Controller Manager |
                               |  etcd               |
                               +---------------------+
                                           |
      ---------------------------------------------------------------------------
          |                                  |                           |
+-------------------+            +---------------------+        +--------------------+
| Worker Node 1     |            | Worker Node 2       |        | Worker Node 3      |
|-------------------|            |---------------------|        |--------------------|
| kubelet           |            | kubelet             |        | kubelet            |
| kube-proxy        |            | kube-proxy          |        | kube-proxy         |
| Container Runtime |            | Container Runtime   |        | Container Runtime  |
| Pods              |            | Pods                |        | Pods               |
+-------------------+            +---------------------+        +--------------------+
```

---

## **4. Kubernetes Example**

### Scenario: Deploy a web app

1. Create a **deployment** file `webapp-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: webapp
  template:
    metadata:
      labels:
        app: webapp
    spec:
      containers:
      - name: webapp
        image: nginx:latest
        ports:
        - containerPort: 80
```

2. Deploy in K8s cluster:

```bash
kubectl apply -f webapp-deployment.yaml
```

3. Expose it as a **service** to access via a browser:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: webapp-service
spec:
  type: NodePort
  selector:
    app: webapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

```bash
kubectl apply -f webapp-service.yaml
```

✅ Now, your Nginx app is running in 3 replicas, load-balanced, and can scale easily.

---

If you want, I can also **draw a full-color visual diagram** showing the **K8s architecture with Pods, Services, and Master-Worker interactions**, which is much easier to understand than text.

Do you want me to do that?

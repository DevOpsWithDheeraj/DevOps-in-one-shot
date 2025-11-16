

# 🚀 **Kubernetes Cluster & Node Management**

Managing a Kubernetes cluster means efficiently handling:

* **Nodes** (machines that run workloads)
* **Namespaces** (logical partitions within a cluster)
* **ResourceQuota** (limits for a namespace)
* **LimitRange** (default and max/min resource limits per Pod/Container)

These concepts help ensure **multi-tenancy, resource fairness, and cluster stability**.

---

# 🖥️ **1. Nodes**

Nodes are the **worker machines** in a Kubernetes cluster.
A node can be:

* **Physical machine**
* **Virtual machine**
* **Cloud-managed node (EKS/GKE/AKS)**

Each node contains:

| Component             | Role                                    |
| --------------------- | --------------------------------------- |
| **Kubelet**           | Manages containers on the node          |
| **Kube-Proxy**        | Handles networking/routing for services |
| **Container Runtime** | e.g., containerd, Docker                |
| **Node OS**           | Linux/Windows                           |

### ✔️ Node States

* `Ready`
* `NotReady`
* `Unknown`
* `SchedulingDisabled` (cordon/drain)
* `DiskPressure`, `MemoryPressure`

---

### ✔️ Basic Node Commands

```bash
kubectl get nodes
kubectl describe node worker-1
kubectl cordon worker-1         # Prevent scheduling
kubectl drain worker-1 --ignore-daemonsets
kubectl uncordon worker-1
```

---

# 🗂️ **2. Namespaces**

Namespaces create **virtual clusters** inside a physical cluster.
They help with:

* Multi-tenancy
* Team/project isolation
* Separate resource boundaries
* Separate access control (RBAC)

### ✔️ Default namespaces

| Namespace         | Purpose                  |
| ----------------- | ------------------------ |
| `default`         | Default workspace        |
| `kube-system`     | System components        |
| `kube-public`     | Publicly readable config |
| `kube-node-lease` | Heartbeats for nodes     |

### ✔️ Create a namespace

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

Apply:

```bash
kubectl apply -f ns-dev.yaml
```

---

# 📦 **3. ResourceQuota**

**ResourceQuota** limits the **aggregate resource usage** of a namespace.

This prevents one team/application from consuming all cluster resources.

### Resources you can limit:

* Total CPU
* Total Memory
* Total number of Pods
* Number of PVCs
* Number of Services
* ConfigMaps
* Secrets

---

### ✔️ Example: ResourceQuota for namespace `dev`

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-quota
  namespace: dev
spec:
  hard:
    requests.cpu: "2"
    requests.memory: 4Gi
    limits.cpu: "4"
    limits.memory: 8Gi
    pods: "10"
    persistentvolumeclaims: "5"
```

👉 Meaning:

* Max **10 Pods**
* Max **2 CPU requests** and **4 CPU limits**
* Max **4Gi memory request** and **8Gi memory limit**
* Max **5 PVCs**

This ensures **fair use** of resources in multi-team setups.

---

# ⚖️ **4. LimitRange**

**LimitRange** controls **per-Pod or per-Container** resource constraints inside a namespace.

It defines:

* Minimum CPU/Memory
* Maximum CPU/Memory
* Default CPU/Memory limits (if not provided by user)
* Default requests

---

### ✔️ Example: LimitRange for namespace `dev`

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: dev-limitrange
  namespace: dev
spec:
  limits:
  - type: Container
    max:
      cpu: "1"
      memory: 1Gi
    min:
      cpu: 100m
      memory: 128Mi
    default:
      cpu: 500m
      memory: 512Mi
    defaultRequest:
      cpu: 200m
      memory: 256Mi
```

### Explanation:

* A container **must request at least**:

  * **CPU:** 100m
  * **Memory:** 128Mi
* A container **cannot exceed**:

  * **CPU:** 1 core
  * **Memory:** 1Gi
* If user does NOT specify resources:

  * Kubernetes assigns **default request**: `200m CPU, 256Mi`
  * Kubernetes assigns **default limit**: `500m CPU, 512Mi`

👉 This ensures **no pod is deployed without proper CPU/Memory** (preventing cluster crashes).

---

# 🔗 **How they work together (Hierarchy)**

```
Cluster
 ├── Nodes (Machines)
 ├── Namespaces (Logical Groups)
 │     ├── ResourceQuota (Namespace-wide limit)
 │     └── LimitRange   (Per pod/container limit)
 └── Workloads (Pod, RS, Deployment...)
```

---

# 🎯 **Complete Example (End-to-End Flow)**

### Scenario:

The **dev team** needs a namespace with:

* Max **10 pods**
* Each container must request min 100m CPU
* Pods cannot exceed 1 CPU limit

### Steps:

1. Create Namespace → `dev`
2. Apply ResourceQuota → restrict namespace total resources
3. Apply LimitRange → enforce pod-level constraints

After this:

* If a developer tries to deploy 20 pods → **blocked**
* If they deploy a pod without resources → **auto assigned**
* If they request 4 CPU → **rejected**
* If namespace runs out of quota → **new pods blocked**

---

# ✅ Summary Table

| Concept           | Purpose                                | Scope               |
| ----------------- | -------------------------------------- | ------------------- |
| **Node**          | Workloads run here                     | Cluster level       |
| **Namespace**     | Logical cluster partition              | Namespace level     |
| **ResourceQuota** | Total resources allowed in a namespace | Namespace level     |
| **LimitRange**    | Min/Max & default resources for pods   | Pod/container level |

---

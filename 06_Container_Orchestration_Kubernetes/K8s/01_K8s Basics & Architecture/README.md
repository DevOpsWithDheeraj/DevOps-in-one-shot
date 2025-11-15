
# ⭐ **What is Kubernetes (K8s)?**

**Kubernetes is an open-source container orchestration platform** that automates:

* Deployment of applications
* Scaling (up/down automatically)
* Load balancing
* Self-healing (restart failed apps)
* Rolling updates
* Resource management

K8s helps you run **hundreds or thousands of containers** smoothly in production.

---

# 🌱 **Why do we need Kubernetes?**

Without K8s:

❌ If a container crashes → You must restart manually <br>
❌ If traffic increases → You must add containers manually <br>
❌ Load balancing must be configured manually <br>
❌ Hard to update apps without downtime <br>
❌ Hard to manage multiple containers <br>

With K8s:

✔ Auto-restarts containers <br>
✔ Auto-scales containers using HPA <br>
✔ Auto load-balancing <br>
✔ Zero-downtime deployments <br>
✔ Rollback support <br>
✔ Manages configuration & secrets <br>
✔ Works across clusters (cloud/on-prem) <br>

---

# 🧱 **K8s Architecture**

```
                        +-------------------------+
                        |      Control Plane      |
                        |-------------------------|
                        |  API Server             |
                        |  etcd                   |
                        |  Scheduler              |
                        |  Controller Manager     |
                        +------------+------------+
                                     |
        ---------------------------------------------------------------
            |                        |                              |
     +---------------+     +---------------+            +---------------+
     |   Worker 1    |     |   Worker 2    |            |   Worker 3    |
     |---------------|     |--------------- |           |---------------|
     | kubelet       |     | kubelet        |           | kubelet       |
     | kube-proxy    |     | kube-proxy     |           | kube-proxy    |
     | containerd    |     | containerd     |           | containerd    |
     | Pods/Containers|    | Pods/Containers|           | Pods/Containers|
     +---------------+     +---------------+            +---------------+
```

Kubernetes architecture has **two main components**:

## **1️⃣ Control Plane (Master Node)**

Brains of Kubernetes – manages everything.

## **2️⃣ Worker Nodes**

Machines where containers (Pods) actually run.

---

# 🧠 **1. Control Plane Components**

### **1. API Server (kube-apiserver)**

* Entry point of the cluster
* Every request (kubectl, internal component) passes through API server
* REST interface of Kubernetes

**Example:**
When you run:

```
kubectl create -f deployment.yaml
```

API server receives and stores it.

---

### **2. etcd (Key-Value Store)**

* Database of your cluster
* Stores desired state

**Example stored in etcd:**

* Number of replicas
* Pod names, IPs
* ConfigMaps, Secrets

---

### **3. Scheduler (kube-scheduler)**

* Decides **which node** will run the Pod.
* Based on CPU, memory, taints, affinity.

**Example:**
3 worker nodes:

* Node1: 60% CPU used
* Node2: 20% used
* Node3: 80% used

Scheduler picks **Node2**.

---

### **4. Controller Manager**

Runs controllers like:

* Node Controller
* Replication Controller
* Deployment Controller
* Job Controller
* Endpoint Controller

**Example:**
You want 3 Pods running.
One Pod dies.
Replication Controller sees mismatch → creates new pod automatically.

---

# 🖥 **2. Worker Node Components**

Worker nodes actually run the applications (Pods).

---

### **1. Kubelet**

* Talks to API server
* Ensures pods are running
* Reports node health

**Example:**
API server → “Start nginx pod”
Kubelet → Pulls image → Starts container.

---

### **2. Kube-Proxy**

* Creates networking routes
* Maintains load balancing inside cluster

**Example:**
If Pod A wants to talk to Pod B, kube-proxy makes it work.

---

### **3. Container Runtime**

Software that actually runs containers.

Examples:

* Docker
* containerd
* CRI-O

---

---

# 🔷 **How Kubernetes Works – HIGH-LEVEL FLOW**

1. You create a deployment:

```
kubectl apply -f nginx-deploy.yaml
```

2. API Server receives it
3. Stores it in **etcd**
4. Scheduler selects best Node
5. Kubelet on that Node creates Pod
6. Pod runs your containers
7. Kube-proxy manages networking
8. If pod fails → Controller creates a new one (self-healing)

---

# 📦 **Kubernetes Core Objects (Basics)**

## **1️⃣ Pod**

Smallest deployable unit in K8s.
Contains 1 or more containers.

**Example pod.yaml**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: app
      image: nginx
```

---

## **2️⃣ Deployment**

Manages **replicas + rolling updates**.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: nginx
          image: nginx:latest
```

---

## **3️⃣ Service**

Exposes Pods.

### Types:

* ClusterIP (default)
* NodePort
* LoadBalancer

**Example:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-svc
spec:
  type: NodePort
  selector:
    app: web
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007
```

---

## **4️⃣ ConfigMap & Secret**

Store configuration.

---

## **5️⃣ PersistentVolume & PVC**

Storage for database apps.

---

## **6️⃣ Namespace**

Logical grouping of resources.

---

# 🚀 **Example Workflow**

### You deploy a 3-replica Nginx deployment:

```
kubectl apply -f nginx.yaml
```

1. API server stores request
2. Scheduler chooses nodes
3. Kubelet runs pods
4. Kube-proxy enables service discovery
5. Deployment controller ensures 3 replicas are always running
6. You update image → Rolling update happens
7. If a pod crashes → New one created automatically

---

# 🎤 **Interview-Safe Summary**

“Kubernetes is a distributed container orchestration system with a **control plane** (API Server, etcd, scheduler, controller manager) and **worker nodes** (kubelet, kube-proxy, container runtime). It manages the lifecycle of containerized applications through objects like Pods, Deployments, Services, and ConfigMaps. It automatically handles scheduling, scaling, load balancing, and self-healing, ensuring reliable and scalable application delivery.”

---


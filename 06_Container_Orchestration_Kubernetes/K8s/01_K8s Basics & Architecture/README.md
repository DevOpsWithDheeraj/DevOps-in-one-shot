

# **1. What is Kubernetes? 🤖☸️**

Kubernetes (K8s) is an open-source container orchestration platform that helps you deploy, manage, scale, and operate containerized applications automatically.

In simple words:
> Kubernetes = A system that runs your containers automatically and keeps them healthy, balanced, and always available.

---
🧩 Why do we need Kubernetes? <br>
Without Kubernetes, running containers manually becomes difficult: <br>
* You need to start/stop containers manually <br>
* No automatic restart if a container fails <br>
* No automatic scaling when traffic increases <br>
* Hard to roll out updates without downtime <br>
> Kubernetes solves all these problems. <br>
---

🔥 Key Features of Kubernetes <br>
1. **Automatic Deployment** : Runs your containers on different machines (nodes). <br>
2. **Self-Healing** : If a container crashes, K8s restarts it.  If a node fails, it moves pods to another node.
3. **Auto Scaling** : Adds or removes containers based on CPU, RAM, or custom metrics.
4. **Load Balancing & Service Discovery** : Kubernetes provides a stable IP address to your app and balances traffic.
5. **Rolling Updates & Rollbacks** :  Update apps without downtime.
6. **Configuration & Secrets Management** : Uses ConfigMaps and Secrets for external configuration.

**Example:**
> You have a web application running in Docker containers. Instead of manually managing 10 containers across 3 servers, K8s handles deployment, scaling, and health checks automatically. ⚡

# **2. Kubernetes Architecture Diagram 🏛️**

```
                               +------------------------+
                               |    Master Node 🖥️     |
                               |----------------------  |
                               |  API Server 🔑        |
                               |  Scheduler 📌         |
                               |  Controller Manager 🔄|
                               |  etcd 💾              |
                               +------------------------+
                                           |
      ---------------------------------------------------------------------------
          |                                  |                           |
+----------------------+            +-----------------------+        +-----------------------+
| Worker Node 1 🧑‍💻    |            | Worker Node 2 🧑‍💻      |        | Worker Node 3 🧑‍💻     |
|----------------------|            |-----------------------|        |------------------ ----|
| kubelet ✅          |            | kubelet ✅            |        | kubelet ✅           |
| kube-proxy 🌐       |            | kube-proxy 🌐         |        | kube-proxy 🌐        |
| Container Runtime 🐳|            | Container Runtime 🐳  |        | Container Runtime 🐳 |
| Pods 📦             |            | Pods 📦               |        | Pods 📦              |
+----------------------+            +-----------------------+        +-----------------------+
```

Kubernetes follows a **Master-Node / Control Plane – Worker Nodes** architecture.

There are **two main parts**:

1. **Control Plane (Master Components)**
2. **Worker Node (Data Plane Components)**

---
## 🧠 **1. CONTROL PLANE (Master Components)**

The **Control Plane** is the "brain" of Kubernetes.
It makes decisions about scheduling, maintaining cluster state, scaling, and responding to failures.

### **Control Plane Components:**

---
### 🟩 **a. API Server (`kube-apiserver`)**

The API Server is the central management component of Kubernetes.

It acts as the **front door** to the entire cluster.

> API Server is the main control-plane component that exposes the Kubernetes API, receives all cluster requests, validates them, and updates the cluster state in etcd.

Every operation in Kubernetes goes through the API Server.

📌 **Example:**
> When you run:

```bash
kubectl create -f deployment.yaml
```

> The request first goes to the **API Server**, which stores the deployment info in etcd.

---

### 🟦 **b. etcd (Key-Value Store)**

etcd is a distributed, reliable, key-value store used by Kubernetes to store all cluster data and configuration.

In short:
> etcd is the database of Kubernetes that stores the entire cluster’s state and configuration.

* A **distributed database** that stores the entire cluster state.
* Stores: pods, deployments, nodes, configmaps, secrets, events, etc.

📌 **Example:**
> If a Pod crashes, Kubelet reads from etcd (via API server) to check desired state.
---

### 🟨 **c. Controller Manager (`kube-controller-manager`)**

The Controller Manager is a control plane component that runs controllers, which are background processes that ensure the cluster’s **desired state** matches the **actual state**.

Main controllers:
* Node Controller
* Deployment Controller
* ReplicaSet Controller
* Endpoint Controller
* Job Controller

📌 **Example:**
> Deployment needs **3 replicas**, but only 2 are running → ReplicaSet controller creates 1 more Pod.

---
### 🟪 **d. Scheduler (`kube-scheduler`)**
The Scheduler is a control plane component that assigns newly created pods to nodes in the cluster based on available resources and constraints.

In short:
> Scheduler = The “placement officer” of Kubernetes that decides which node a pod should run on.

* Assigns Pods to nodes based on resources, taints, affinity, etc.

📌 **Example:**
> A Pod needing 2 CPU & 2Gi RAM will be scheduled to a node that has enough free resources.

---
### 🟧 **e. Cloud Controller Manager (optional)**

* Integrates Kubernetes with cloud providers (AWS, GCP, Azure).

📌 **Example:**
> Creates AWS load balancers when you create a `LoadBalancer` service.

---

## 🖥️ **2. WORKER NODES (Data Plane)**

Worker Nodes run your applications (Pods).

Each worker node has:


### 🔵 **a. Kubelet**

* Agent running on every node.
* Ensures containers are running as per the PodSpec.

📌 **Example:**
> If API Server says a Pod must run here, kubelet pulls the image and starts the container.

---
### 🔴 **b. Kube-Proxy**

* Handles cluster networking & load-balancing for services.
* Manages communication between Pods and Services.

📌 **Example:**
> You hit a NodePort → kube-proxy forwards traffic to the correct Pod.

---
### 🟢 **c. Container Runtime**

Runs containers (Docker, containerd, CRI-O).

> 📌 **Containers inside Pods are run using this runtime.**

---


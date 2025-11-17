

## **1. What is Kubernetes? 🤖☸️**

Kubernetes (K8s) is an **open-source container orchestration platform** that automates the deployment, scaling, and management of containerized applications.

**Why use Kubernetes?**

* 🚀 Automatically deploys containers across multiple servers (nodes).
* 🛠️ Self-healing: Restarts failed containers, replaces unhealthy ones.
* 📈 Horizontal scaling: Adds or removes containers based on demand.
* 🌐 Service discovery & load balancing.

**Example:**

> You have a web application running in Docker containers. Instead of manually managing 10 containers across 3 servers, K8s handles deployment, scaling, and health checks automatically. ⚡

## **2. Kubernetes Architecture Diagram 🏛️**

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

# 🧠 **1. CONTROL PLANE (Master Components)**

The **Control Plane** is the "brain" of Kubernetes.
It makes decisions about scheduling, maintaining cluster state, scaling, and responding to failures.

### **Control Plane Components:**

---

## 🟩 **a. API Server (`kube-apiserver`)**

* The **entry point** to the Kubernetes cluster.
* All kubectl commands go to the API server.
* Validates requests and updates the cluster state.

📌 **Example:**
When you run:

```bash
kubectl create -f deployment.yaml
```

The request first goes to the **API Server**, which stores the deployment info in ETCD.

---

## 🟦 **b. etcd (Key-Value Store)**

* A **distributed database** that stores the entire cluster state.
* Stores: pods, deployments, nodes, configmaps, secrets, events, etc.

📌 **Example:**
If a Pod crashes, Kubelet reads from etcd (via API server) to check desired state.

---

## 🟨 **c. Controller Manager (`kube-controller-manager`)**

Contains controllers that watch the cluster state and ensure desired state = current state.

Main controllers:

* Node Controller
* Deployment Controller
* ReplicaSet Controller
* Endpoint Controller
* Job Controller

📌 **Example:**
Deployment needs **3 replicas**, but only 2 are running → ReplicaSet controller creates 1 more Pod.

---

## 🟪 **d. Scheduler (`kube-scheduler`)**

* Assigns Pods to nodes based on resources, taints, affinity, etc.

📌 **Example:**
A Pod needing 2 CPU & 2Gi RAM will be scheduled to a node that has enough free resources.

---

## 🟧 **e. Cloud Controller Manager (optional)**

* Integrates Kubernetes with cloud providers (AWS, GCP, Azure).

📌 **Example:**
Creates AWS load balancers when you create a `LoadBalancer` service.

---

---

# 🖥️ **2. WORKER NODES (Data Plane)**

Worker Nodes run your applications (Pods).

Each worker node has:

---

## 🔵 **a. Kubelet**

* Agent running on every node.
* Ensures containers are running as per the PodSpec.

📌 **Example:**
If API Server says a Pod must run here, kubelet pulls the image and starts the container.

---

## 🔴 **b. Kube-Proxy**

* Handles cluster networking & load-balancing for services.
* Manages communication between Pods and Services.

📌 **Example:**
You hit a NodePort → kube-proxy forwards traffic to the correct Pod.

---

## 🟢 **c. Container Runtime**

Runs containers (Docker, containerd, CRI-O).

📌 **Containers inside Pods are run using this runtime.**

---


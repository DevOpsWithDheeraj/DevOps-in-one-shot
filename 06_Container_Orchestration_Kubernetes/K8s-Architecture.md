

## 🌐 What is Kubernetes?

**Kubernetes (K8s)** is an **open-source container orchestration platform** developed by Google.
It automates the **deployment, scaling, and management** of containerized applications.

Think of Kubernetes as the **brain of your container infrastructure** — it decides:

* Where containers should run,
* How many should run,
* How to restart them if they fail, and
* How to connect them securely.

---

## ⚙️ High-Level Architecture

Kubernetes follows a **Master–Worker (Control Plane–Node)** architecture.

```
+---------------------------------------------------+
|                   Control Plane                   |
|  (API Server | etcd | Controller | Scheduler)     |
+---------------------------------------------------+
                |
                |   (Communicates via kubelet)
                v
+---------------------------------------------------+
|                      Node(s)                      |
|  (kubelet | kube-proxy | Container Runtime | Pods)|
+---------------------------------------------------+
```

---

## 🧠 1. Control Plane (Master Components)

The **Control Plane** manages the cluster.
It makes global decisions (like scheduling and scaling) and detects/responds to cluster events (like restarting a failed pod).

### 🧩 Components:

### 1️⃣ **API Server (`kube-apiserver`)**

* Acts as the **entry point** to the Kubernetes cluster.
* All communication (kubectl, UI, or other components) happens through the API Server.
* It exposes **REST APIs**.

> 🧠 Think of it as the **“front desk”** of Kubernetes — all requests go through here.

**Example:**
When you run `kubectl apply -f deployment.yaml`, the command talks to the API Server.

---

### 2️⃣ **etcd**

* It’s a **distributed key-value database** that stores all **cluster state and configuration**.
* Stores info like: pods, secrets, config maps, nodes, etc.
* It’s **highly available** and **consistent**.

> 🧠 Think of it as **Kubernetes’ memory** — it remembers everything about the cluster.

---

### 3️⃣ **Controller Manager (`kube-controller-manager`)**

* Runs **controllers**, which are background processes that ensure the **desired state == current state**.

📦 Examples of controllers:

* **Node Controller** – manages node health.
* **Replication Controller** – ensures correct number of pod replicas.
* **Endpoint Controller** – manages service endpoints.
* **Namespace Controller** – handles namespaces.

> 🧠 Think of it as **the manager that makes sure things go as planned**.

---

### 4️⃣ **Scheduler (`kube-scheduler`)**

* Decides **which node** a pod should run on.
* It looks at:

  * Node resources (CPU, Memory)
  * Pod requirements
  * Node affinity/taints/tolerations
  * Workload balance

> 🧠 Think of it as a **smart dispatcher** that finds the right worker for each job.

---

## ⚙️ 2. Worker Nodes (Node Components)

Worker nodes actually **run the containers**.

Each node has several components:

---

### 1️⃣ **kubelet**

* An **agent** running on each node.
* Communicates with the **API Server**.
* Ensures that the **pods** described in the control plane are running on the node.

> 🧠 Think of it as a **node’s personal assistant** — it keeps containers running as told by the master.

---

### 2️⃣ **kube-proxy**

* Maintains **network rules** on the node.
* Handles **service discovery** and **load balancing** between pods.
* It routes traffic to the correct pod inside the cluster.

> 🧠 Think of it as a **network router** for your node.

---

### 3️⃣ **Container Runtime**

* Responsible for **running containers**.
* Examples:

  * Docker (earlier)
  * containerd
  * CRI-O
* Kubernetes uses **Container Runtime Interface (CRI)** to talk to the runtime.

> 🧠 Think of it as the **engine** that actually runs containers.

---

### 4️⃣ **Pods**

* The **smallest deployable unit** in Kubernetes.
* A Pod can have one or more containers.
* All containers in a pod share the same:

  * Network namespace (same IP)
  * Storage volumes

> 🧠 Think of a Pod as a **wrapper around containers** that defines how they run together.

---

## ⚙️ How Kubernetes Works (Step-by-Step Flow)

Let’s say you deploy an app using:

```bash
kubectl apply -f deployment.yaml
```

### Step 1️⃣ — User Request

* The command sends the deployment manifest to the **API Server**.

### Step 2️⃣ — Validation & Storage

* API Server **validates** the request.
* The desired state is stored in **etcd**.

### Step 3️⃣ — Scheduler Action

* The **Scheduler** sees new pods waiting to be scheduled.
* It picks suitable **nodes** and assigns pods to them.

### Step 4️⃣ — Kubelet Execution

* The **kubelet** on the chosen node notices the assigned pods.
* It pulls the container images and starts them via the **container runtime**.

### Step 5️⃣ — Controller Monitoring

* **Controller Manager** continuously monitors the system.
* If a pod crashes, it creates a new one to maintain the desired state.

### Step 6️⃣ — Networking

* **kube-proxy** sets up networking rules.
* Services and DNS help route traffic to the correct pod.

---

## 🧱 Summary Table

| Component          | Type          | Function                          |
| ------------------ | ------------- | --------------------------------- |
| API Server         | Control Plane | Entry point, handles all requests |
| etcd               | Control Plane | Stores cluster state              |
| Scheduler          | Control Plane | Assigns pods to nodes             |
| Controller Manager | Control Plane | Ensures desired state             |
| Kubelet            | Node          | Manages pods on node              |
| Kube-proxy         | Node          | Handles networking                |
| Container Runtime  | Node          | Runs containers                   |

---

## 🔄 Example: Real-World Flow

**Scenario:** You deploy an Nginx web app.

1. `kubectl apply -f nginx-deployment.yaml`
2. API Server accepts and stores the configuration in etcd.
3. Scheduler assigns pods to available nodes.
4. Kubelet on that node starts Nginx container.
5. Kube-proxy sets up networking for external access.
6. If the pod crashes, Controller Manager creates a new one automatically.

---

## 🧩 In Short

**Kubernetes Architecture =**
👉 *Control Plane (brains)* + *Worker Nodes (muscles)*
that continuously talk to maintain your **desired state**.

---

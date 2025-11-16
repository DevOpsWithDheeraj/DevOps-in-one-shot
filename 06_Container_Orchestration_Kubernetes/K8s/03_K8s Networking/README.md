

# 🚀 **Kubernetes Networking**

Kubernetes networking is built on **4 golden rules**:

1️⃣ **Every Pod gets its own unique IP** <br>
2️⃣ **All Pods can communicate with all other Pods (flat network)** <br>
3️⃣ **Node IPs and Pod IPs are different** <br>
4️⃣ **Pods are ephemeral → Pod IPs keep changing** <br>

To handle this, Kubernetes provides:

✔ **Services** (Stable IP) <br>
✔ **Ingress** (Smart L7 routing) <br>
✔ **CNI Plugins** (Pod networking) <br>
✔ **Network Policies** (Firewall rules) <br>

---

# 1️⃣ **Service Networking in Kubernetes**

A **Service** provides a **stable, permanent virtual IP (ClusterIP)** for a set of Pods.

Pods may die → new Pod gets a new IP,
but the **Service IP never changes**.

There are **3 main Service types**:

---

# 🔹 1. ClusterIP (Default Service)

In Kubernetes, ClusterIP is one of the Service types used to expose a set of Pods internally within the cluster. It is the default Service type. 

A **ClusterIP service** provides a **virtual IP address** (called **ClusterIP**) inside the Kubernetes cluster. This IP is only reachable **within the cluster**, not from the outside world.

> **Purpose:** Allows Pods and other services inside the cluster to communicate with each other using a stable IP, even if the Pods themselves change.


## **Example YAML**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: ClusterIP
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80       # Service port
      targetPort: 8080 # Pod port
```

* **`selector`** → chooses the Pods this service will route traffic to.
* **`port`** → the port on which the service is exposed internally.
* **`targetPort`** → the port on the Pod receiving traffic.
* **`type: ClusterIP`** → makes it accessible only inside the cluster.

💡 **Tip:** If you want the service to be accessible from **outside the cluster**, you would use `NodePort` or `LoadBalancer` instead of `ClusterIP`.

---

# 🔹 2. NodePort Service

In Kubernetes, **NodePort** is another **Service type** that allows you to expose your application **outside the cluster**. 

A **NodePort service** exposes a service on a **static port on each Node** of the Kubernetes cluster. This allows external clients to access your service using:

```
<NodeIP>:<NodePort>
```

* **NodeIP** → IP of any node in the cluster
* **NodePort** → port assigned by Kubernetes (default range: **30000–32767**) or manually defined.

## **How it works**

1. Kubernetes assigns a **ClusterIP** (internal IP) to the service, just like ClusterIP.
2. It also allocates a **NodePort**.
3. Traffic coming to `<NodeIP>:<NodePort>` is forwarded to the **ClusterIP**, which then routes it to the matching Pods.

This means NodePort is basically **ClusterIP + external exposure via node port**.

## **Example YAML**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80         # Service port
      targetPort: 8080 # Pod port
      nodePort: 30080  # Optional: static NodePort, else Kubernetes assigns one
```

* **`port`** → internal cluster port
* **`targetPort`** → pod port receiving traffic
* **`nodePort`** → port on all cluster nodes exposed externally
* **`type: NodePort`** → enables access from outside

💡 **Tip:** NodePort is simple but not ideal for production in large clusters, because exposing many NodePorts can get messy. For production, **LoadBalancer** is preferred.

---

# 🔹 3. LoadBalancer Service

In Kubernetes, a **LoadBalancer** is a **Service type** that exposes your application to the **external world** with the help of a cloud provider's load balancer. 

A **LoadBalancer service** creates an **external IP** that routes traffic to your Kubernetes service.

* It uses the cloud provider’s (AWS, GCP, Azure) native load balancer.
* Internally, it still uses a **ClusterIP**, so traffic is routed to Pods via the cluster network.

### **How it works**

1. You create a Service with `type: LoadBalancer`.
2. Kubernetes requests the cloud provider to provision a **load balancer**.
3. The cloud load balancer receives traffic on a public IP and forwards it to the **NodePort**, which then forwards to the **ClusterIP**, and finally to the **Pods**.

### **Example YAML**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer-service
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80         # External port
      targetPort: 8080 # Pod port
```

* **`port`** → port exposed externally (via LoadBalancer)
* **`targetPort`** → port on the Pod
* **`type: LoadBalancer`** → triggers external load balancer creation

💡 **Extra Tip:**

* In **bare-metal clusters** (no cloud), LoadBalancer won’t work out-of-the-box; you’d need solutions like **MetalLB** to simulate this functionality.
---

# 2️⃣ **Ingress – Layer 7 (HTTP/HTTPS Routing)**

LoadBalancer is good for ONE service.
But 20 services → 20 load balancers = 💸 expensive.

Ingress solves this.

### ✔ Features

* One LoadBalancer for many services
* Domain-based routing
* Path-based routing
* TLS termination

### Example Routing

```
example.com/      → frontend
example.com/api   → backend
example.com/auth  → auth-service
```

### YAML

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: webapp-ingress
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: frontend-service
            port:
              number: 80
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: backend-service
            port:
              number: 8080
```

### Diagram

```
Internet
   ↓
Cloud LoadBalancer
   ↓
Ingress Controller (NGINX/Traefik/Istio)
   ↓
Routes to correct services
```

---

# 3️⃣ **CNI – Container Network Interface**

Kubernetes itself does NOT create Pod networking.
CNI plugins do that.

### ✔ CNI Responsibilities

1️⃣ Assign Pod IP
2️⃣ Create veth pairs
3️⃣ Setup routing between nodes
4️⃣ Overlay / Underlay networking
5️⃣ Implement Network Policies

### Popular CNI Plugins

| CNI               | Highlights                   |
| ----------------- | ---------------------------- |
| **Calico**        | Network policy, BGP, fast    |
| **Flannel**       | Very simple, overlay (VXLAN) |
| **Weave**         | Automatic mesh, encryption   |
| **Cilium (eBPF)** | High performance, security   |

---

# 🔹 What Happens When a Pod is Created?

Example:

Pod gets IP → `10.244.2.15`
Steps:

* CNI assigns IP
* Creates veth pair
* Attaches Pod to bridge
* Configures routing
* Updates cluster routing map

---

# 4️⃣ **Network Policies – Kubernetes Firewall**

By default:

✔ All Pods can talk to all Pods
❌ No security between microservices

Network Policies enforce **zero-trust networking**.

**Supported by:** Calico, Cilium, Weave
(Not supported by basic Flannel)

---

## 🔸 NetworkPolicy Concepts

A NetworkPolicy defines:

✔ **Which Pods are selected**
✔ **What incoming/outgoing traffic is allowed**
✔ **Ports**
✔ **Sources/Destinations**

---

# ✔ Example 1: Allow Only Frontend → Backend

### Goal

Only frontend can talk to backend on port 8080.

### YAML

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend-to-backend
spec:
  podSelector:
    matchLabels:
      app: backend
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: frontend
      ports:
        - protocol: TCP
          port: 8080
```

### Result

```
Frontend → Backend:8080   ✔ ALLOWED
Database → Backend:8080   ✖ BLOCKED
Any Pod → Backend         ✖ BLOCKED
```

---

# ✔ Example 2: Allow Backend → Database (Egress Policy)

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: backend-egress-to-db
spec:
  podSelector:
    matchLabels:
      app: backend
  policyTypes:
    - Egress
  egress:
    - to:
        - podSelector:
            matchLabels:
              app: database
      ports:
        - protocol: TCP
          port: 3306
```

---

# 🔥 Full Networking Flow Diagram (Complete)

```
                   Internet
                      ↓
          +------------------------+
          |   Cloud LoadBalancer   |
          +------------------------+
                      ↓
            +--------------------+
            |  Ingress Controller |
            +--------------------+
              ↓       ↓       ↓
        /      /api      /auth
        ↓        ↓         ↓
frontend-svc  backend-svc  auth-svc
   ↓             ↓            ↓
frontend pods  backend pods  auth pods
   ↓             ↓            ↓
NetworkPolicy  NetworkPolicy  NetworkPolicy
   ↓             ↓            ↓
      (Traffic allowed only where defined)
```

---

# 🧠 **Final Summary Table**

| Component         | Layer | Purpose                    |
| ----------------- | ----- | -------------------------- |
| **ClusterIP**     | L4    | Internal-only service      |
| **NodePort**      | L4    | Expose service via node IP |
| **LoadBalancer**  | L4    | External cloud LB          |
| **Ingress**       | L7    | Domain/path routing        |
| **CNI Plugin**    | L2/L3 | Pod networking, routing    |
| **NetworkPolicy** | L3/L4 | Pod-level firewall         |

---

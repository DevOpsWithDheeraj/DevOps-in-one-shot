

# 🚀 **Kubernetes Networking**

Kubernetes networking is built on **4 golden rules**:

1️⃣ **Every Pod gets its own unique IP**
2️⃣ **All Pods can communicate with all other Pods (flat network)**
3️⃣ **Node IPs and Pod IPs are different**
4️⃣ **Pods are ephemeral → Pod IPs keep changing**

To handle this, Kubernetes provides:

✔ **Services** (Stable IP)
✔ **Ingress** (Smart L7 routing)
✔ **CNI Plugins** (Pod networking)
✔ **Network Policies** (Firewall rules)

---

# 1️⃣ **Service Networking in Kubernetes**

A **Service** provides a **stable, permanent virtual IP (ClusterIP)** for a set of Pods.

Pods may die → new Pod gets a new IP,
but the **Service IP never changes**.

There are **3 main Service types**:

---

# 🔹 1. ClusterIP (Default Service)

ClusterIP exposes your application **inside the cluster only**.

### 📌 When to use?

* Backend ↔ Database communication
* Internal microservice communication

### Example

```
http://backend-service:8080
```

Even if Pods restart, service always routes to correct Pods using labels.

### Diagram

```
Frontend Pod → ClusterIP Service → Backend Pods
```

### YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  selector:
    app: backend
  ports:
  - port: 8080
    targetPort: 8080
```

---

# 🔹 2. NodePort Service

Exposes your application **on each node’s IP at a static port (30000–32767)**.

### 📌 When to use?

* Minikube / Kind
* No cloud load balancer
* Simple external access

### Access Format

```
http://NodeIP:NodePort
```

### Diagram

```
Client → NodeIP:NodePort → ClusterIP → Pods
```

### YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: webapp-nodeport
spec:
  type: NodePort
  selector:
    app: webapp
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080
```

---

# 🔹 3. LoadBalancer Service

Creates a **cloud provider Load Balancer** (AWS, GCP, Azure).

### 📌 When to use?

* Production
* Public-facing apps
* HTTP / TCP apps needing external IP

### Access Format

```
http://External-LB-IP
```

### Diagram

```
Internet → Cloud Load Balancer → NodePort → ClusterIP → Pods
```

### YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: webapp-lb
spec:
  type: LoadBalancer
  selector:
    app: webapp
  ports:
  - port: 80
    targetPort: 80
```

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

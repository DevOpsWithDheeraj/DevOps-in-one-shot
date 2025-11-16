

# 🚀 **Kubernetes Networking**

Kubernetes networking is built on 4 golden rules:

1️⃣ **Every Pod gets its own unique IP** <br>
2️⃣ **All Pods can communicate with all other Pods (flat network)** <br>
3️⃣ **Node IPs and Pod IPs are separate** <br>
4️⃣ **Pods are ephemeral → IPs change** <br>
So… Kubernetes provides **Services**, **Ingress**, and **CNI plugins** to solve networking challenges. <br>

---

# 1️⃣ **Service Networking in Kubernetes**

A **Service** provides a **stable, permanent virtual IP (ClusterIP)** for a group of Pods.

Pods may die and get recreated → new IPs,
But the **Service IP remains constant**.

There are 3 main Service types:

### ✔ **ClusterIP**

### ✔ **NodePort**

### ✔ **LoadBalancer**

---

# 🔹 1. ClusterIP (Default Service)

ClusterIP exposes your application **inside the cluster only**.

### 📌 When to use?

* Communication between **backend ↔ database**
* Internal microservices need to talk to each other

### 📌 Example

Frontend can call backend using:

```
http://backend-service:8080
```

Even if backend pods change IPs, the Service always routes traffic to the correct pods using **labels**.

### 🔹 How it works:

```
Frontend Pod → ClusterIP Service → Backend Pods
```

### YAML Example

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

NodePort exposes your application **on each node’s IP at a static port (30000–32767)**.

### 📌 When to use?

* Local cluster (Minikube, Kind)
* No cloud load balancer available
* Simple external access for testing

### Access Format

```
http://NodeIP:NodePort
```

Example:

```
http://192.168.1.10:30080
```

### 🔹 How it works:

```
Client → NodeIP:NodePort → ClusterIP Service → Pods
```

### YAML Example

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

LoadBalancer exposes your Service to the **public internet** using the **cloud provider’s LB**
(e.g., AWS ELB, GCP LB, Azure LB).

### 📌 When to use?

* Production workloads
* Need internet-facing app
* Auto-assign external IP

### Access Format

```
http://ExternalLoadBalancerIP
```

### 🔹 How it works:

```
Internet → Cloud Load Balancer → NodePort → ClusterIP → Pods
```

### YAML Example

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

# 2️⃣ **Ingress – Layer 7 (HTTP/HTTPS) Routing**

A **LoadBalancer** is fine for 1 service.
But imagine 20 services → 20 load balancers = **expensive**.

Ingress solves this.

### ✔ Ingress = Smart HTTP/HTTPS router

✔ Works at **application layer (layer 7)**
✔ Exposes multiple services using **ONE LoadBalancer**

---

# 🔹 Example Use Case

You want:

* `/` → frontend service
* `/api` → backend service
* `/auth` → authentication service

### Ingress Routing

```
example.com/      → frontend-service
example.com/api   → backend-service
example.com/auth  → auth-service
```

### Ingress YAML Example

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

### 🔹 How Ingress Works:

```
Internet
   ↓
Cloud Load Balancer (1 LB only)
   ↓
Ingress Controller (NGINX/Traefik/Istio)
   ↓
Routes to correct Service
```

---

# 3️⃣ **CNI – Container Network Interface**

Kubernetes does NOT implement low-level networking itself.
Instead, it uses **CNI plugins** to create Pod networking.

### ✔ **CNI = How Pods get IPs & how routing is built**

---

# Popular CNI Providers

### 1. **Calico**

* L3 routing
* Network Policies
* Production-grade
* Cloud + on-prem

### 2. **Flannel**

* Simple overlay network
* VXLAN-based
* Easy to set up

### 3. **Weave**

* Auto mesh networking
* Simple encryption

### 4. **Cilium (eBPF-based)**

* High performance
* Advanced security
* Great for production

---

# 🔹 What CNIs Actually Do?

CNI is responsible for:
1️⃣ Assigning IP to pods
2️⃣ Creating virtual ethernet pairs
3️⃣ Routing packets between nodes
4️⃣ Handling overlay/underlay networks
5️⃣ Enforcing network policies

### Example of Pod Networking

When a Pod is created:

* CNI assigns Pod IP → `10.244.3.25`
* Creates a **veth pair**

  * One end in Pod
  * One end in host network namespace
* Configures routing rules
* Updates cluster-wide routing via CNI backend (Flannel/Calico/etc.)

---

# 🔥 Bringing it all Together (Full Flow Diagram)

```
Internet
   ↓
+------------------------+
|  Cloud LoadBalancer    |  ← (For LoadBalancer & Ingress)
+------------------------+
          ↓
   +---------------------+
   |  Ingress Controller |
   +---------------------+
        ↓        ↓
   /api → backend-service (ClusterIP)
   /    → frontend-service (ClusterIP)
        ↓                     ↓
   backend Pods         frontend Pods
        ↓                     ↓
      (CNI: Calico/Flannel assigns pod IPs)
        ↓
   Pod-to-Pod communication (flat network)
```

---

# 🧠 **Summary Table**

| Component        | Layer | Purpose                      |
| ---------------- | ----- | ---------------------------- |
| **ClusterIP**    | L4    | Internal communication       |
| **NodePort**     | L4    | Access via node IP           |
| **LoadBalancer** | L4    | Cloud external access        |
| **Ingress**      | L7    | Domain + path based routing  |
| **CNI**          | L2/L3 | Pod networking, routing, IPs |

---


In **DevOps**, especially in **Kubernetes**, the term **Ingress** refers to a **Kubernetes resource** that manages **external access to services** running inside a Kubernetes cluster — typically **HTTP and HTTPS traffic**.

Think of **Ingress** as a **smart traffic controller** or **entry gate** for your cluster.

---

## 🔹 Why Ingress Is Needed

When you deploy applications in Kubernetes:

* Each application runs inside Pods.
* Pods are exposed via **Services** (like ClusterIP, NodePort, or LoadBalancer).
* Without Ingress, every service would need a **separate external IP** — which is inefficient.

**Ingress solves this problem** by:

* Allowing **multiple services** to be accessed through **a single IP and domain**.
* Handling **routing, SSL termination, and load balancing**.

---

## 🔹 How Ingress Works

1. A user sends a request from the browser (e.g., `https://shop.example.com`).
2. The request hits the **Ingress Controller** (e.g., NGINX Ingress, AWS ALB Ingress).
3. The Ingress Controller checks the **Ingress rules**.
4. Based on the rules, it forwards traffic to the correct **Kubernetes Service**.

---

## 🔹 Ingress Example

### 🧩 Suppose you have two apps:

* **frontend-service** (React UI)
* **backend-service** (API)

You can create one Ingress to route based on path or host:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /frontend
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
              number: 80
```

✅ **What happens:**

* `https://myapp.example.com/frontend` → routes to `frontend-service`
* `https://myapp.example.com/api` → routes to `backend-service`

---

## 🔹 Ingress Controller Example

Ingress resources need a **controller** to function — they don’t route traffic by themselves.

Popular controllers:

* **NGINX Ingress Controller**
* **AWS ALB Ingress Controller**
* **Traefik**
* **Istio Gateway (for service mesh)**

To install NGINX Ingress Controller (example):

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

---

Here are **more clear & practical examples of Kubernetes Ingress** that you can use in interviews, real DevOps work, and learning.

✅ Host-based routing
✅ Path-based routing
✅ Single service routing
✅ Multiple-domain Ingress
✅ TLS/HTTPS example
✅ Ingress with rewrite rules
✅ Ingress for microservices
✅ Ingress load balancing behavior

---

# ✅ **1. Simple Ingress Example (Single Service)**

**Goal:** Route traffic from `myapp.example.com` → `my-service`

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: simple-ingress
spec:
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-service
            port:
              number: 80
```

🔍 **Use Case:**
Your client wants the application accessible by a single domain.

---

# ✅ **2. Host-Based Routing (Different Domains → Different Services)**

**Goal:**

* `api.example.com` → API service
* `ui.example.com` → UI service

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: host-routing-ingress
spec:
  rules:
  - host: api.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: api-service
            port:
              number: 80

  - host: ui.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: ui-service
            port:
              number: 80
```

🔍 **Use Case:**
Best for **microservices** where each service gets its own domain.

---

# ✅ **3. Path-Based Routing (One Domain → Multiple Services)**

**Goal:**
One domain but route based on URLs:

* `/frontend` → frontend-service
* `/backend` → backend-service

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: path-routing-ingress
spec:
  rules:
  - host: shop.example.com
    http:
      paths:
      - path: /frontend
        pathType: Prefix
        backend:
          service:
            name: frontend-service
            port:
              number: 80

      - path: /backend
        pathType: Prefix
        backend:
          service:
            name: backend-service
            port:
              number: 80
```

🔍 **Use Case:**
UI + API inside one domain.

---

# ✅ **4. Ingress With TLS/HTTPS (SSL Certificate)**

This enables **HTTPS** on Ingress.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: tls-ingress
spec:
  tls:
  - hosts:
    - secure.example.com
    secretName: tls-secret
  rules:
  - host: secure.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: secure-service
            port:
              number: 80
```

🔍 **Use Case:**
For production deployments with SSL.

📌 **You must create the secret first:**

```bash
kubectl create secret tls tls-secret \
  --cert=cert.crt \
  --key=cert.key
```

---

# ✅ **5. Ingress With Rewrite Rules**

Example: incoming request `/app` → service root `/`

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: rewrite-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: demo.example.com
    http:
      paths:
      - path: /app
        pathType: Prefix
        backend:
          service:
            name: app-service
            port:
              number: 80
```

🔍 **Use Case:**
For legacy apps that don’t support subpath routing.

---

# ✅ **6. Multiple Ingress Resources for Same Ingress Controller**

You can create separate ingress files for different teams.

### Team A:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: team-a-ingress
spec:
  rules:
  - host: a.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: team-a-service
            port:
              number: 80
```

### Team B:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: team-b-ingress
spec:
  rules:
  - host: b.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: team-b-service
            port:
              number: 80
```

🔍 **Use Case:**
Multi-team/multitenant Kubernetes clusters.

---

# ✅ **7. Wildcard Domain Ingress**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: wildcard-ingress
spec:
  rules:
  - host: "*.example.com"
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: default-service
            port:
              number: 80
```

🔍 **Use Case:**
All subdomains → one app.

---

# ✅ **8. Ingress for Microservices Architecture**

**Architecture Example:**

| Path       | Service         |
| ---------- | --------------- |
| `/user`    | user-service    |
| `/order`   | order-service   |
| `/payment` | payment-service |
| `/cart`    | cart-service    |

### Ingress:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ecommerce-ingress
spec:
  rules:
  - host: shop.example.com
    http:
      paths:
      - path: /user
        pathType: Prefix
        backend:
          service:
            name: user-service
            port:
              number: 80

      - path: /order
        pathType: Prefix
        backend:
          service:
            name: order-service
            port:
              number: 80

      - path: /payment
        pathType: Prefix
        backend:
          service:
            name: payment-service
            port:
              number: 80

      - path: /cart
        pathType: Prefix
        backend:
          service:
            name: cart-service
            port:
              number: 80
```

🔍 **Use Case:**
Complete microservices-based e-commerce platform.

---

# ✅ **9. Ingress Load Balancing Example**

If your service has **multiple pods**, Ingress will automatically load balance:

```bash
kubectl scale deployment api-deploy --replicas=4
```

Ingress will:

* Spread traffic across 4 pods
* Health-check pods
* Automatically remove unhealthy pods

No extra config required — Ingress handles it.

---

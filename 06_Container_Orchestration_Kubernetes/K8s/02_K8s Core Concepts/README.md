

# 🚀 **Kubernetes Core Concepts**

## 1️⃣ **POD — The Smallest Deployable Unit**

A **Pod** is the **smallest and simplest deployable unit** in Kubernetes that represents **one or more containers** running together with shared storage, network, and configuration.

### 🔍 **Key Points About Pods:** <br>
* Smallest deployable object in Kubernetes  <br>
* Runs **containers** (usually 1 container, sometimes more) <br>
* Containers inside a Pod **share:** <br>
           * Same network namespace (same IP, same ports) <br>
           * Same storage volumes <br>
           * Same lifecycle <br>
* Designed to run **a single instance** of an application <br>

**Note**: 
> **Pod = 1 or more containers + shared network + shared storage**


### 🎯 Example (one-container Pod)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: myapp-container
    image: nginx:latest
```

💡 **Use-case**: Running a small Nginx web server.

---

## 2️⃣ **ReplicaSet — Ensures Desired Number of Pods**

A ReplicaSet is a Kubernetes controller that ensures a specified number of identical Pods are always running.
If a Pod crashes, gets deleted, or becomes unhealthy, the ReplicaSet automatically creates a new one to maintain the desired count.

**Notes:**
> ReplicaSet = Desired number of Pods + Auto-healing

### 🎯 Example

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: nginx
        image: nginx
```

💡 **Use-case**: You want **3 replicas** of your app for load-balancing & reliability.

---

## 3️⃣ **Deployment — The Boss of ReplicaSets**

A Deployment in Kubernetes is a high-level controller used to manage Pods and provide built-in features like **auto-healing**, **auto-scaling**, and **rolling updates**.

A Deployment ensures that the desired number of pod replicas are always running and healthy.

You can think of: <br>
> Deployment = Pods + Auto-healing + Auto-scaling + Version Control (Rolling Updates/Rollbacks)

### 🎯 Example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: nginx
        image: nginx:1.20
```

💡 **Use-case**: You want to update your app from nginx 1.20 → 1.21 **without downtime**
K8s will roll pods one by one.

🔁 Workflow: <br>
> Deployment → ReplicaSet → Pods → (Auto Healing + Auto Scaling + Rolling Updates)
---

## 4️⃣ **Service — Expose & Route Traffic to Pods**

A Service in Kubernetes is a logical abstraction that provides a stable network endpoint to access one or more Pods.

Even if Pods die, restart, or change IP addresses, the Service gives a consistent IP, DNS name, and port to reach them.

### Types of Services

| Type                | Purpose                            |
| ------------------- | ---------------------------------- |
| ClusterIP (default) | Accessible **inside cluster only** |
| NodePort            | Exposes service on **node's port** |
| LoadBalancer        | Exposes to **internet** (cloud)    |

---

### 🎯 Example: ClusterIP Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  selector:
    app: myapp
  ports:
  - port: 80
    targetPort: 80
```

💡 **Use-case**: Internal communication such as
Frontend → Backend service

---

### 🎯 Example: NodePort Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-nodeport
spec:
  type: NodePort
  selector:
    app: myapp
  ports:
  - port: 80         # Port exposed by the service
    targetPort: 80    # Port on the Pod/Container
    nodePort: 30080    # Port on the Node
```

💡 **Use-case**: Access from browser using:
`<NodeIP>:30080`

---

## 5️⃣ **ConfigMap — Externalizing Non-Sensitive Configurations**

A ConfigMap in Kubernetes is an object used to store non-confidential configuration data (key–value pairs) separately from application code.

It allows you to inject configuration into Pods without rebuilding the container image.

### 🎯 Example ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: myapp-config
data:
  APP_ENV: production
  APP_COLOR: blue
```

### Use ConfigMap in Deployment

```yaml
env:
- name: APP_ENV
  valueFrom:
    configMapKeyRef:
      name: myapp-config
      key: APP_ENV
```

💡 **Use-case**: Changing app environment **without rebuilding images**.

---

## 6️⃣ **Secret — Stores Sensitive Data**

A Secret in Kubernetes is an object used to store sensitive data such as passwords, tokens, SSL keys, and credentials in a secure and encoded form.

It keeps confidential information separate from Pod specifications and prevents hardcoding sensitive values inside container images or YAML files..

### 🎯 Example Secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: myapp-secret
type: Opaque
data:
  DB_PASSWORD: cGFzc3dvcmQxMjM=   # base64 of password123
```

### Use Secret in Deployment

```yaml
env:
- name: DB_PASSWORD
  valueFrom:
    secretKeyRef:
      name: myapp-secret
      key: DB_PASSWORD
```

💡 **Use-case**: Storing DB password securely.

---

# 🔗 How All Components Work Together (Real Example)

### Scenario: You deploy an Nginx-based web app

| K8s Component  | Role                                     |
| -------------- | ---------------------------------------- |
| **Pod**        | Runs your app container                  |
| **ReplicaSet** | Ensures 3 Pods are always available      |
| **Deployment** | Controls updates, rollbacks, scaling     |
| **Service**    | Exposes app to internal/external network |
| **ConfigMap**  | Stores app settings like theme, URLs     |
| **Secret**     | Stores DB username/password              |

### Flow

1. Deployment creates a ReplicaSet
2. ReplicaSet creates 3 Pods
3. Service exposes Pods
4. Pods read configs from ConfigMap
5. Pods read secrets securely from Secret

---

# 🧠 Summary Table

| Concept        | Purpose              | Example            |
| -------------- | -------------------- | ------------------ |
| **Pod**        | Runs container       | nginx pod          |
| **ReplicaSet** | Maintain pod count   | 3 replicas         |
| **Deployment** | App lifecycle mgmt   | Rolling update     |
| **Service**    | Stable networking    | NodePort/ClusterIP |
| **ConfigMap**  | Non-sensitive config | APP_ENV=prod       |
| **Secret**     | Sensitive data       | DB password        |

---

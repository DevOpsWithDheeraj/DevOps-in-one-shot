
# 🚀 **Kubernetes Deployment Strategies**

Deployment strategies define **how new versions of your application are released** with minimum downtime and risk.

Kubernetes supports multiple strategies:

1. **Recreate**
2. **Rolling Update** (default)
3. **Blue-Green Deployment**
4. **Canary Deployment**

Let’s understand them in detail with diagrams and examples.

---

# 1️⃣ **Recreate Deployment Strategy**

### 📌 What is Recreate?

The **existing version is completely stopped first**, and then the **new version is deployed**.

### 🔥 When to Use?

* When you CANNOT run two versions at the same time.
  (Examples: DB schema incompatible, apps that hold long-lived sessions.)
* Simple deployments where downtime is acceptable.

### ⚠️ Disadvantage

* Causes **downtime** because old pods are killed first.

---

### 📘 Example YAML (Recreate Strategy)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  strategy:
    type: Recreate
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myapp:v2
```

### 🔄 Flow

1. Kubernetes kills all existing pods.
2. New pods (v2) start after all old pods are gone.

### 📌 Diagram

Old Pods (v1): 🟥🟥🟥
Shutdown → downtime
New Pods (v2): 🟩🟩🟩 (start)

---

# 2️⃣ **Rolling Update (Default Strategy)**

### 📌 What is Rolling Update?

Pods are **updated gradually**, one batch at a time.
No downtime → smooth upgrade.

### 🔥 When to Use?

* Most common production deployments.
* Stateless applications.
* Applications supporting multiple versions.

### ✔️ Advantages

* Zero downtime
* Controlled rollout
* Can roll back quickly

---

### 📘 Example YAML (Rolling Update Strategy)

```yaml
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
```

### 🔄 Flow

* Out of 3 pods:

  * Kill 1 old pod → create 1 new pod
  * Repeat

### 📌 Diagram

Before: 🟥🟥🟥
During: 🟥🟥🟩 → 🟥🟩🟩 → 🟩🟩🟩
After: 🟩🟩🟩

---

# 3️⃣ **Blue-Green Deployment**

### 📌 What is Blue-Green?

You run **two complete environments**:

* **Blue (current version)**
* **Green (new version)**

Traffic is switched from Blue→Green **instantly** once Green is validated.

### 🔥 When to Use?

* Production-grade deployments
* When reliability and rollback speed is critical
* Used in AWS, GCP, Azure pipelines

### ✔️ Advantages

* Zero downtime
* Fast rollback
* Test Green version before release

### ⚠️ Disadvantages

* Costs more resources (2 full environments)

---

### 📘 Example (Concept)

**Blue Namespace**

* Deployment v1 → service `myapp-svc` → Prod traffic

**Green Namespace**

* Deployment v2 → service `myapp-svc-green`

When tested:

```bash
kubectl patch svc myapp-svc -p '{"spec":{"selector":{"version":"v2"}}}'
```

Traffic instantly switches to Green.

### 📌 Diagram

Blue (v1): 🟦🟦🟦 (receiving traffic)
Green (v2): 🟩🟩🟩 (standby/testing)
Switch → Green receives traffic
Blue stays as backup

---

# 4️⃣ **Canary Deployment**

### 📌 What is Canary?

Release new version (v2) to **a small percentage of users**, while majority continues using v1.

Example traffic split:

* v1 → 90%
* v2 → 10%

If stable → gradually increase to 50%, 100%

### 🔥 When to Use?

* When new release risk is high
* To test real-world traffic
* Popular with Istio / Linkerd / Argo Rollouts

### ✔️ Advantages

* Real user testing
* Safe gradual rollout
* Easy rollback

---

### 📘 Kubernetes Example (using labels)

Deployment v1:

```yaml
labels:
  app: myapp
  version: v1
```

Deployment v2:

```yaml
labels:
  app: myapp
  version: v2
```

Service (initially pointing to v1):

```yaml
selector:
  app: myapp
  version: v1
```

Using service mesh or ingress, you set traffic rules:

Example (Istio VirtualService):

```yaml
spec:
  http:
  - route:
    - destination:
        host: myapp
        subset: v1
      weight: 90
    - destination:
        host: myapp
        subset: v2
      weight: 10
```

Later update: v2 → 50%

---

### 📌 Diagram

Traffic →
90%: 🟥 (v1)
10%: 🟩 (v2)

If healthy → increase → 100% v2

---

# 🆚 Summary Comparison

| Strategy           | Downtime | Cost   | Risk     | Rollback  | Best For           |
| ------------------ | -------- | ------ | -------- | --------- | ------------------ |
| **Recreate**       | Yes      | Low    | Medium   | Normal    | Simple apps        |
| **Rolling Update** | No       | Low    | Low      | Easy      | Most apps          |
| **Blue-Green**     | No       | High   | Very Low | Instant   | Critical apps      |
| **Canary**         | No       | Medium | Very Low | Very Easy | High-risk releases |

---



## 🚀 **What is a Deployment in Kubernetes?**

A **Deployment** in Kubernetes is a **controller** that manages Pods and ReplicaSets.
It ensures your application is **running**, **scalable**, and **recoverable** automatically.

In simple words:

> A Deployment tells Kubernetes *how many replicas (Pods)* of an application you want, and *how they should be updated or replaced*.

---

## 🧠 **Why Do We Need a Deployment?**

If you create Pods manually:

* They won’t **auto-restart** if they crash.
* They can’t **scale** easily.
* Updating them **requires deleting and recreating** them.

💡 A **Deployment** solves all this:

* Creates & manages Pods automatically.
* Maintains a **ReplicaSet** to ensure the desired number of Pods are running.
* Supports **rolling updates** and **rollbacks**.

---

## 🧩 **How a Deployment Works**

```
Deployment
   ↓
ReplicaSet (manages replicas)
   ↓
Pods (containers actually running)
```

If a Pod fails, the ReplicaSet replaces it automatically — maintaining the desired state.

---

## ⚙️ **Example 1: Basic NGINX Deployment**

Here’s a simple **Deployment YAML**:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3  # number of Pods
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
```

---

### 🧾 **Create the Deployment**

```bash
kubectl apply -f nginx-deployment.yaml
```

### 🧾 **Check Deployment**

```bash
kubectl get deployments
```

### 🧾 **Check ReplicaSets (auto-created)**

```bash
kubectl get rs
```

### 🧾 **Check Pods**

```bash
kubectl get pods
```

You’ll see **3 Pods** running (since replicas = 3).

---

## 🔁 **Rolling Updates (Zero Downtime)**

When you update the image version in your Deployment YAML (for example, `nginx:1.25` instead of `nginx:latest`) and apply it again:

```bash
kubectl apply -f nginx-deployment.yaml
```

Kubernetes will:

* Create **new Pods** with the updated image.
* Gradually **terminate old Pods**.
* Ensure the app stays available during the update.

✅ This is called a **rolling update**.

---

## 🔙 **Rollback to Previous Version**

If the new deployment has an issue:

```bash
kubectl rollout undo deployment nginx-deployment
```

Check rollout status:

```bash
kubectl rollout status deployment nginx-deployment
```

---

## ⚡ **Scaling a Deployment**

You can increase or decrease replicas dynamically:

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

Now, 5 Pods will run.

---

## 💥 **Deleting a Deployment**

```bash
kubectl delete deployment nginx-deployment
```

This also deletes all associated Pods and ReplicaSets.

---

## 🧰 **Useful Deployment Commands**

| Command                                        | Description                  |
| ---------------------------------------------- | ---------------------------- |
| `kubectl get deploy`                           | List all deployments         |
| `kubectl describe deploy <name>`               | Detailed info                |
| `kubectl rollout status deploy <name>`         | See rollout progress         |
| `kubectl rollout history deploy <name>`        | See version history          |
| `kubectl rollout undo deploy <name>`           | Rollback to previous version |
| `kubectl scale deploy <name> --replicas=<num>` | Scale replicas               |
| `kubectl delete deploy <name>`                 | Delete deployment            |

---

## ⚙️ **Example 2: Deployment with Environment Variables & Resource Limits**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: webapp
  template:
    metadata:
      labels:
        app: webapp
    spec:
      containers:
        - name: webapp
          image: node:18-alpine
          ports:
            - containerPort: 3000
          env:
            - name: ENVIRONMENT
              value: "production"
          resources:
            requests:
              cpu: "250m"
              memory: "256Mi"
            limits:
              cpu: "500m"
              memory: "512Mi"
```

This Deployment:

* Runs 2 Pods.
* Exposes port 3000.
* Sets environment variable `ENVIRONMENT=production`.
* Defines CPU/memory **requests and limits**.

---

## 🧠 **Real-World Example**

You can have multiple Deployments in a project:

| Environment | Deployment Name   | Purpose                      |
| ----------- | ----------------- | ---------------------------- |
| `dev`       | `frontend-deploy` | Frontend for dev testing     |
| `staging`   | `backend-deploy`  | Backend for QA validation    |
| `prod`      | `nginx-deploy`    | Production load-balanced app |

Each can run in different **namespaces**, with different **replica counts**, and **resource limits**.

---

## ✅ **Summary**

| Concept             | Description                                  |
| ------------------- | -------------------------------------------- |
| **Deployment**      | Controller that manages ReplicaSets and Pods |
| **Purpose**         | Automate app updates, scaling, and recovery  |
| **Key Features**    | Rolling updates, rollbacks, scaling          |
| **Components**      | Deployment → ReplicaSet → Pods               |
| **Command Example** | `kubectl apply -f deployment.yaml`           |

---

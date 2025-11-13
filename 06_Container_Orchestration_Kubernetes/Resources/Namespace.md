
## 🧱 **What is a Namespace in Kubernetes (K8s)?**

A **Namespace** in Kubernetes is a **logical partition** or **virtual cluster** within a physical Kubernetes cluster.

It allows you to **isolate** and **organize** resources (like Pods, Services, Deployments, etc.) so multiple teams or environments can work within the same cluster **without interfering** with each other.

---

## 🎯 **Why Do We Need Namespaces?**

Namespaces help in:

1. **Resource Isolation** --
   Different teams or projects can deploy their workloads independently.

2. **Access Control (RBAC)** --
   You can apply different permissions using **Role** and **RoleBinding** per namespace.

3. **Resource Management** --
   You can limit CPU/memory usage for each namespace using **ResourceQuota** and **LimitRange**.

4. **Organization** --
   Keeps your cluster clean — resources grouped logically by environment or application.

---

## 🧩 **Default Namespaces in Kubernetes**

When you create a new cluster, Kubernetes comes with some **default namespaces**:

| Namespace           | Purpose                                                |
| ------------------- | ------------------------------------------------------ |
| **default**         | Used for resources that don’t specify a namespace.     |
| **kube-system**     | Used by system components (like kube-dns, kube-proxy). |
| **kube-public**     | Publicly readable data, usually for cluster info.      |
| **kube-node-lease** | Used for node heartbeat tracking.                      |

---

## ⚙️ **Example: Creating and Using a Namespace**

### 🧾 Step 1: Create a Namespace

**YAML Manifest:**

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev-namespace
```

**Command to apply:**

```bash
kubectl apply -f namespace.yaml
```

**OR directly from CLI:**

```bash
kubectl create namespace dev-namespace
```

---

### 🧾 Step 2: Deploy a Pod or Deployment in that Namespace

**deployment.yaml**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
  namespace: dev-namespace
spec:
  replicas: 2
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

**Command:**

```bash
kubectl apply -f deployment.yaml
```

---

### 🧾 Step 3: Verify Namespace and Resources

List all namespaces:

```bash
kubectl get ns
```

List Pods in a specific namespace:

```bash
kubectl get pods -n dev-namespace
```

List Deployments in that namespace:

```bash
kubectl get deploy -n dev-namespace
```

---

### 🧾 Step 4: Set Default Namespace for kubectl (Optional)

To avoid typing `-n dev-namespace` every time:

```bash
kubectl config set-context --current --namespace=dev-namespace
```

Now all your `kubectl` commands will default to that namespace.

---

## 🧠 **Real-World Example**

Let’s say your company runs multiple environments:

* `dev` → Developers test features
* `staging` → QA verifies before release
* `prod` → Live environment for users

You can create separate namespaces for each:

```bash
kubectl create ns dev
kubectl create ns staging
kubectl create ns prod
```

Each namespace can have its own:

* Deployments and Services
* Resource limits (CPU/Memory)
* Access controls (RBAC)

---

## ✅ **Summary**

| Concept               | Description                                                |
| --------------------- | ---------------------------------------------------------- |
| **Namespace**         | Logical isolation within a cluster                         |
| **Purpose**           | Organize, isolate, manage access & resources               |
| **Default Namespace** | `default`, `kube-system`, `kube-public`, `kube-node-lease` |
| **Example**           | `kubectl create ns dev-namespace`                          |
| **Use Case**          | Separate environments: dev, staging, prod                  |

---

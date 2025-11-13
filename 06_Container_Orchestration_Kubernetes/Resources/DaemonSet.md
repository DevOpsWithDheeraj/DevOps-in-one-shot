

## ⚙️ **What is a DaemonSet in Kubernetes?**

A **DaemonSet** ensures that a **copy of a Pod runs on every node** (or on specific nodes) in a Kubernetes cluster.

Whenever a new node is added, Kubernetes **automatically schedules the DaemonSet Pod** on it.
When a node is removed, the Pod running on that node is also deleted.

---

### 🧠 **In Simple Terms:**

> A **DaemonSet** makes sure that *one Pod of a specific type* runs on *every node* — like a background agent or system daemon.

---

## 💡 **Why Do We Need DaemonSets?**

DaemonSets are used for running **cluster-level background services** such as:

* **Log collection agents** → e.g., Fluentd, Filebeat
* **Monitoring agents** → e.g., Prometheus Node Exporter
* **Security agents** → e.g., Falco
* **Storage daemons** → e.g., Ceph, GlusterFS

---

## 🧱 **Key Characteristics**

| Feature                   | Description                                     |
| ------------------------- | ----------------------------------------------- |
| **One Pod per node**      | Ensures 1 Pod runs on each node                 |
| **Auto-scheduling**       | New nodes automatically get a Pod               |
| **Auto-cleanup**          | When a node leaves, the Pod is deleted          |
| **Cluster-wide services** | Used for monitoring, logging, networking agents |

---

## 📄 **Example: DaemonSet YAML**

Here’s an example of a DaemonSet that runs an **NGINX Pod on all nodes**:

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-daemonset
  labels:
    app: nginx
spec:
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

### 🧾 **Apply the DaemonSet**

```bash
kubectl apply -f nginx-daemonset.yaml
```

### 🧾 **Verify DaemonSet**

```bash
kubectl get ds
```

Example output:

```
NAME               DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
nginx-daemonset    3         3         3       3            3           <none>          2m
```

> This means 3 nodes = 3 Pods (one on each node)

---

### 🧾 **Check Pods**

```bash
kubectl get pods -o wide
```

Output might show:

```
NAME                    READY   STATUS    NODE
nginx-daemonset-abc12   1/1     Running   node1
nginx-daemonset-def34   1/1     Running   node2
nginx-daemonset-ghi56   1/1     Running   node3
```

✅ Each node runs exactly one copy of the DaemonSet Pod.

---

## 🧰 **Common DaemonSet Commands**

| Command                        | Description                     |
| ------------------------------ | ------------------------------- |
| `kubectl get ds`               | List all DaemonSets             |
| `kubectl describe ds <name>`   | Describe DaemonSet details      |
| `kubectl get pods -o wide`     | See DaemonSet Pods on all nodes |
| `kubectl delete ds <name>`     | Delete DaemonSet                |
| `kubectl apply -f <file>.yaml` | Create or update DaemonSet      |

---

## 🧠 **DaemonSet Pod Scheduling Control**

You can control **which nodes** should run the DaemonSet using:

* **nodeSelector**
* **nodeAffinity**
* **tolerations** (for tainted nodes)

### Example — Run only on nodes labeled `role=worker`:

```yaml
spec:
  template:
    spec:
      nodeSelector:
        role: worker
```

---

## 🧩 **DaemonSet vs Deployment**

| Feature                    | DaemonSet               | Deployment            |
| -------------------------- | ----------------------- | --------------------- |
| **Pod per node**           | ✅ Yes (1 per node)      | ❌ No                  |
| **Scaling**                | ❌ Not manually scalable | ✅ Yes                 |
| **Use Case**               | Background agents       | Application workloads |
| **Automatic on new nodes** | ✅ Yes                   | ❌ No                  |
| **Rolling updates**        | ✅ Supported (apps/v1)   | ✅ Supported           |

---

## 🧠 **Real-World Examples**

| Use Case           | Example                      |
| ------------------ | ---------------------------- |
| **Log collection** | Fluentd, Filebeat            |
| **Monitoring**     | Node Exporter, Datadog Agent |
| **Networking**     | Calico, Cilium               |
| **Storage**        | Ceph, GlusterFS Daemons      |

---

## 💥 **Delete DaemonSet**

```bash
kubectl delete ds nginx-daemonset
```

This deletes all DaemonSet-managed Pods.

---

## 🧠 **Summary**

| Concept                | Description                                    |
| ---------------------- | ---------------------------------------------- |
| **DaemonSet**          | Ensures a Pod runs on every (or selected) node |
| **Use Case**           | Background, monitoring, logging services       |
| **Automatic behavior** | Auto-adds Pod on new node                      |
| **Typical Examples**   | Fluentd, Node Exporter, CNI plugins            |
| **Scaling**            | Based on nodes, not replicas                   |
| **Command**            | `kubectl get ds`                               |

---

## 🔍 **Visual Overview**

```
         ┌──────────────────────────────┐
         │        DaemonSet             │
         │ (Ensures Pod per Node)       │
         └─────────────┬────────────────┘
                       │
 ┌──────────────┬──────────────┬──────────────┐
 │              │              │              │
 │   Node 1     │   Node 2     │   Node 3     │
 │ nginx-pod    │ nginx-pod    │ nginx-pod    │
 └──────────────┴──────────────┴──────────────┘
```

---

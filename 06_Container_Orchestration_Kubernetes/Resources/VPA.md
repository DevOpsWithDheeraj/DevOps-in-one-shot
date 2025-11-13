
## ⚙️ **What is VPA in Kubernetes?**

**Vertical Pod Autoscaler (VPA)** automatically **adjusts the CPU and memory requests and limits of Pods** based on usage.

* Unlike HPA (Horizontal Pod Autoscaler), which **scales the number of Pods**,
* VPA **scales the resources of each Pod vertically** (CPU/memory).

---

### 🧠 **In Simple Terms:**

> VPA monitors a Pod’s resource usage and **recommends or applies changes** to CPU and memory so the Pod has **just enough resources** to run efficiently.

---

## 🧩 **Why Do We Need VPA?**

* Avoid **resource starvation** (Pod crashing due to insufficient CPU/memory).
* Avoid **over-provisioning** (wasting cluster resources).
* Useful for **stateful applications** where scaling Pods horizontally is hard (like databases).

---

## ⚡ **VPA Components**

1. **Recommender** – Monitors Pod usage and generates resource recommendations.
2. **Updater** – Updates running Pods by **restarting them with new resources** if needed.
3. **Admission Controller** – Applies recommended resources to **new Pods on creation**.

> VPA requires **VPA Controller** installed in the cluster.

---

## 🔧 **VPA Modes**

| Mode        | Description                                      |
| ----------- | ------------------------------------------------ |
| **Off**     | Only provides **recommendations**, no changes    |
| **Auto**    | Automatically updates Pod resources              |
| **Initial** | Applies recommendations **only at Pod creation** |

---

## 📄 **Example: VPA YAML**

### VPA for Deployment

```yaml
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: nginx-vpa
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx-deployment
  updatePolicy:
    updateMode: "Auto"
  resourcePolicy:
    containerPolicies:
      - containerName: nginx
        minAllowed:
          cpu: 100m
          memory: 128Mi
        maxAllowed:
          cpu: 1
          memory: 1Gi
```

### Explanation:

* `targetRef` → Deployment or StatefulSet to scale resources.
* `updateMode: Auto` → VPA automatically adjusts CPU and memory.
* `minAllowed` / `maxAllowed` → Resource limits for scaling.

---

### Apply VPA:

```bash
kubectl apply -f nginx-vpa.yaml
kubectl get vpa
kubectl describe vpa nginx-vpa
```

---

## 🔄 **How VPA Works**

1. Monitors Pods’ CPU and memory usage.
2. Calculates **recommended requests and limits**.
3. Updates Pod spec (may restart Pod if running).

Example:

| Pod     | CPU request | CPU usage | Memory request | Memory usage |
| ------- | ----------- | --------- | -------------- | ------------ |
| nginx-0 | 200m        | 400m      | 256Mi          | 512Mi        |

* VPA may **increase requests to 400m CPU and 512Mi memory**.
* Pod is restarted (if updateMode=Auto) with new resources.

---

## 🔄 **VPA vs HPA**

| Feature          | VPA                            | HPA                            |
| ---------------- | ------------------------------ | ------------------------------ |
| **Scaling type** | Vertical (resources per Pod)   | Horizontal (# of Pods)         |
| **Use case**     | Stateful apps, DBs             | Stateless apps, web servers    |
| **Pod restarts** | Yes (if running)               | No                             |
| **Metrics**      | CPU, Memory                    | CPU, Memory, Custom            |
| **Combination**  | Can combine with HPA carefully | Can combine with VPA carefully |

> ⚠️ **HPA + VPA together** needs careful configuration to avoid conflicts.

---

## 🔍 **Visual Overview**

```
Deployment / StatefulSet
 └─ Pods
      ├─ CPU: 200m → VPA adjusts to 400m
      └─ Memory: 256Mi → VPA adjusts to 512Mi
```

---

## 🔧 **Common Commands**

| Command                       | Description                |
| ----------------------------- | -------------------------- |
| `kubectl get vpa`             | List VPAs                  |
| `kubectl describe vpa <name>` | Show recommended resources |
| `kubectl delete vpa <name>`   | Delete VPA                 |

---

## ✅ **Summary**

| Concept           | Description                                      |
| ----------------- | ------------------------------------------------ |
| **VPA**           | Vertical Pod Autoscaler                          |
| **Purpose**       | Auto-scale Pod CPU and memory resources          |
| **Modes**         | Off, Auto, Initial                               |
| **Metrics**       | CPU, Memory usage                                |
| **Use Case**      | Stateful apps or apps hard to scale horizontally |
| **Best Practice** | Combine with HPA carefully for complex workloads |

---

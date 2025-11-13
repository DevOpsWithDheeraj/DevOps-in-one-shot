
## ⚙️ **What is HPA in Kubernetes?**

**Horizontal Pod Autoscaler (HPA)** automatically **scales the number of Pods in a Deployment, ReplicaSet, or StatefulSet** based on:

* CPU utilization
* Memory usage
* Custom metrics (like requests per second)

It ensures your application can **handle traffic spikes** without manual intervention.

---

### 🧠 **In Simple Terms:**

> HPA monitors Pod resource usage and **increases or decreases the number of Pods dynamically** to match demand.

---

## 🧩 **Why Do We Need HPA?**

* Handle **traffic spikes** automatically.
* Avoid **over-provisioning resources**.
* Maintain **application performance** and **availability**.
* Reduce **costs** in cloud environments.

---

## ⚡ **HPA Workflow**

```
Metrics (CPU/Memory/Custom) → HPA → Adjust `# of Pods`
```

Steps:

1. HPA monitors metrics via **Metrics Server**.
2. Compares metrics with target thresholds.
3. Scales up or down the number of Pods automatically.

---

## 🔧 **Prerequisites**

* Metrics Server must be installed in the cluster:

```bash
kubectl get deployment metrics-server -n kube-system
```

If not installed, you can deploy it from:

```
https://github.com/kubernetes-sigs/metrics-server
```

---

## 📄 **Example: HPA YAML**

### Deploy a Deployment first:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
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
          image: nginx
          resources:
            requests:
              cpu: 100m
            limits:
              cpu: 500m
```

---

### Create HPA for CPU-based scaling:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: nginx-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx-deployment
  minReplicas: 2
  maxReplicas: 5
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50
```

---

### Explanation:

* `scaleTargetRef` → HPA will scale this Deployment.
* `minReplicas` → Minimum number of Pods.
* `maxReplicas` → Maximum number of Pods.
* `metrics` → HPA will scale Pods to maintain **CPU usage at 50%**.

---

### Apply HPA:

```bash
kubectl apply -f nginx-hpa.yaml
kubectl get hpa
```

Output:

```
NAME        REFERENCE             TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
nginx-hpa   Deployment/nginx      20%/50%   2         5         2          1m
```

* `TARGETS` shows **current CPU / desired CPU**.
* `REPLICAS` shows **current number of Pods**.

---

## 🔧 **Simulate Load**

1. Install a load generator, e.g., `stress` in another Pod.
2. Generate CPU load on `nginx-deployment`.
3. HPA will automatically **increase Pods up to maxReplicas**.
4. Once load drops, HPA scales **back down**.

---

## 🔄 **HPA vs VPA vs Cluster Autoscaler**

| Feature     | HPA                           | VPA                           | Cluster Autoscaler |
| ----------- | ----------------------------- | ----------------------------- | ------------------ |
| **Scale**   | Horizontal (Pods)             | Vertical (CPU/Memory of Pods) | Nodes in cluster   |
| **Trigger** | Metrics (CPU, memory, custom) | Resource requests/limits      | Node utilization   |
| **Purpose** | Auto-scale Pods               | Auto-adjust Pod resources     | Auto-scale nodes   |

---

## 🧰 **Common HPA Commands**

| Command                                                                | Description        |
| ---------------------------------------------------------------------- | ------------------ |
| `kubectl get hpa`                                                      | List HPAs          |
| `kubectl describe hpa <name>`                                          | Show HPA details   |
| `kubectl delete hpa <name>`                                            | Delete HPA         |
| `kubectl autoscale deployment <name> --cpu-percent=50 --min=2 --max=5` | Quick HPA creation |

---

## 🔍 **Visual Overview**

```
             ┌─────────────┐
             │  Metrics    │
             │ CPU / Memory│
             └─────┬───────┘
                   │
                   ▼
            ┌─────────────┐
            │     HPA     │
            │ Horizontal  │
            │ Pod Scaling │
            └─────┬───────┘
                  │
       ┌──────────┴──────────┐
       │                     │
   Deployment             Deployment
    Pods                    Pods
(scale up/down based on metrics)
```

---

## ✅ **Summary**

| Concept             | Description                                                           |
| ------------------- | --------------------------------------------------------------------- |
| **HPA**             | Horizontal Pod Autoscaler                                             |
| **Purpose**         | Automatically scale Pods based on metrics                             |
| **Metrics**         | CPU, Memory, Custom Metrics                                           |
| **Scaling**         | minReplicas → maxReplicas                                             |
| **Requirements**    | Metrics Server installed                                              |
| **Command Example** | `kubectl autoscale deployment nginx --cpu-percent=50 --min=2 --max=5` |

---

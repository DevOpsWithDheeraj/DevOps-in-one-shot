
# 🚀 **Kubernetes Monitoring & Autoscaling**

Kubernetes provides multiple layers of autoscaling and monitoring to ensure applications run efficiently and scale based on load.

There are **3 levels of autoscaling**:

1. **Pod-level autoscaling → HPA (Horizontal Pod Autoscaler)**
2. **Pod-level resource tuning → VPA (Vertical Pod Autoscaler)**
3. **Node-level scaling → Cluster Autoscaler**

To support these, Kubernetes uses a monitoring pipeline like:

```
Metrics Server → HPA / VPA  
Custom Metrics Adapter → HPA (custom metrics)
Node metrics → Cluster Autoscaler
```

---

# ⚙️ **1. Metrics Server (Base Monitoring Layer)**

### **What it does**

* Collects **CPU & Memory** usage of:

  * pods
  * nodes
* Provides metrics through **Kube API Aggregation layer**
* Required for **HPA** to work

### **It does NOT store metrics long-term**

(For long-term monitoring you use Prometheus.)

### **Install example**

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

### **Check metrics**

```bash
kubectl top pod
kubectl top node
```

---

# 📈 **2. Horizontal Pod Autoscaler (HPA)**

👉 *Automatically increases/decreases number of PODs*

### **What it scales?**

ReplicaSets / Deployments.

### **When to use?**

* To handle **variable loads**
* Scale based on:

  * CPU
  * memory (using custom metrics)
  * custom metrics (via Prometheus adapter)

### **Example: CPU-based scaling**

Deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: demo
  template:
    metadata:
      labels:
        app: demo
    spec:
      containers:
        - name: demo
          image: nginx
          resources:
            requests:
              cpu: 100m
```

HPA object:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: demo-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: demo-app
  minReplicas: 1
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

👉 If average pod CPU usage > 50% → HPA scales up

---

# 📦 **3. Vertical Pod Autoscaler (VPA)**

👉 *Automatically adjusts CPU/Memory requests of Pods*

### **What it does**

* Sets **right-size resources** for containers
* Prevents:

  * out-of-memory kills
  * CPU throttling
  * over-provisioning

### **Modes**

* `Off` → only recommendations
* `Initial` → sets resources only during pod creation
* `Auto` → evicts and recreates pod with new resources

### **Example VPA**

```yaml
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: demo-vpa
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: demo-app
  updatePolicy:
    updateMode: Auto
```

👉 VPA will observe pod usage → update CPU/memory requests → restart pod.

---

# 🖥️ **4. Cluster Autoscaler**

👉 *Automatically adds/removes **worker nodes** in your cluster.*

### **Supported on clouds**

* AWS (EKS)
* GCP (GKE)
* Azure (AKS)
* Kops
* Cluster API

### **When does it scale up?**

If pods are **Pending** because:

* insufficient CPU
* insufficient memory
* no node matches affinity or taints

### **When does it scale down?**

* Node unused for X minutes
* All pods can fit on fewer nodes

### **Cluster Autoscaler Behavior**

```
More load → HPA adds pods → nodes insufficient → Cluster Autoscaler adds nodes
Less load → pods reduced → nodes empty → Cluster Autoscaler removes nodes
```

---

# 📊 **5. Custom Metrics & External Metrics**

### **Used when you want to scale based on:**

* Requests per second
* Queue depth
* Kafka lag
* Prometheus queries
* External APIs (SQS, CloudWatch)

🎯 HPA supports:

1. **Resource metrics (CPU/Memory)** → via Metrics Server
2. **Custom metrics** → via Prometheus Adapter
3. **External metrics** → via External Metrics Adapter

---

## ✔️ Example: HPA scaling using Prometheus custom metrics

### HPA YAML

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: custom-hpa
spec:
  minReplicas: 1
  maxReplicas: 10
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: demo-app
  metrics:
  - type: Pods
    pods:
      metric:
        name: http_requests_per_second
      target:
        type: AverageValue
        averageValue: 10
```

👉 If RPS > 10 per pod → HPA adds more pods.

---

# 🧠 Putting it All Together (Workflow Diagram)

```
           Metrics Server --- CPU/Memory --> HPA / VPA
                              |
Prometheus <---- Custom Metrics Adapter ---- HPA (custom metrics)
                              |
Cluster Nodes ------------------------------> Cluster Autoscaler
```

---

# ✅ Summary Table

| Component              | Purpose                       | Scales                    | Example Trigger     |
| ---------------------- | ----------------------------- | ------------------------- | ------------------- |
| **Metrics Server**     | Provides CPU/memory metrics   | —                         | kubectl top pod     |
| **HPA**                | Scales Pods horizontally      | Pods (via Deployments/RS) | CPU > 60%           |
| **VPA**                | Adjusts resource requests     | Pod resources             | pod OOM kills       |
| **Cluster Autoscaler** | Scales nodes                  | Worker nodes              | Pending pods        |
| **Custom Metrics**     | Used for custom scaling logic | Pods                      | requests per second |

---

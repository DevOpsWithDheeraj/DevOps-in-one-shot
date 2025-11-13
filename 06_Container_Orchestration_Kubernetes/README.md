# 🧠 What is Kubernetes (K8s)?

Kubernetes is an open-source container orchestration platform developed by Google (now maintained by CNCF).
It automates deployment, scaling, and management of containerized applications.

> You can think of Kubernetes as a “cluster manager” that takes your applications (in containers), runs them efficiently across machines, and ensures they are always healthy.
---

## 🧩 1. Kubernetes Architecture

| Component | Description |
|-----------|-------------|
| **Master Node** | Controls the cluster (API Server, Controller Manager, Scheduler, etcd) |
| **Worker Node** | Runs application containers via kubelet |
| **Pod** | Smallest deployable unit (one or more containers) |
| **Deployment** | Declarative update mechanism for pods |
| **Service** | Stable endpoint for pods; load balancing |
| **Ingress** | HTTP/HTTPS routing to services |
| **ConfigMap / Secret** | Store configuration & sensitive data |
| **Volume** | Persistent storage for pods |

### 🔹 Kubernetes Cluster Components
- **API Server (Port 6443)** – central communication point  
- **etcd** – distributed key-value store for cluster state  
- **Controller Manager** – maintains desired state  
- **Scheduler** – assigns pods to nodes  
- **kubelet** – agent on worker nodes managing pods  
- **kube-proxy** – manages networking rules for pods  

---

## 🧰 2. Kubernetes Networking & Ports

| Port | Purpose |
|------|---------|
| 6443 | API Server |
| 2379-2380 | etcd communication |
| 10250 | kubelet API |
| 30000-32767 | NodePort services |
| 80 / 443 | HTTP/HTTPS traffic to services via Ingress |

**Pod-to-Pod Networking:**  
- Each pod has a **unique IP**  
- **Cluster networking** allows pods to communicate without NAT  
- Services provide **stable IPs** and **DNS names**  

---

## 🔹 3. Kubernetes Basics (Kubectl Commands)

| Command | Purpose | Example |
|---------|---------|---------|
| `kubectl get nodes` | List nodes | `kubectl get nodes` |
| `kubectl get pods` | List pods | `kubectl get pods` |
| `kubectl describe pod <pod>` | Detailed pod info | `kubectl describe pod mypod` |
| `kubectl logs <pod>` | View logs | `kubectl logs mypod` |
| `kubectl exec -it <pod> -- bash` | Access pod shell | `kubectl exec -it mypod -- bash` |
| `kubectl apply -f <file>` | Create/update resources | `kubectl apply -f deployment.yaml` |
| `kubectl delete -f <file>` | Delete resources | `kubectl delete -f deployment.yaml` |

---

## 🐳 4. Deploy a Simple App

### Deployment YAML Example
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
      - name: myapp
        image: dheeraj/node-app:latest
        ports:
        - containerPort: 3000
````

**Deploy:**

```bash
kubectl apply -f deployment.yaml
kubectl get pods
```

### Create Service for External Access

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort
  selector:
    app: myapp
  ports:
    - port: 3000
      targetPort: 3000
      nodePort: 32000
```

Access via `http://<NodeIP>:32000`.

---

## ⚡ 5. Advanced Kubernetes Concepts

### 🔹 Scaling

```bash
kubectl scale deployment myapp-deployment --replicas=5
```

### 🔹 Rolling Updates

```bash
kubectl set image deployment/myapp-deployment myapp=dheeraj/node-app:v2
kubectl rollout status deployment/myapp-deployment
```

### 🔹 ConfigMaps & Secrets

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  ENV: production
```

Mount in pod:

```yaml
envFrom:
  - configMapRef:
      name: app-config
```

### 🔹 Persistent Storage (PVC)

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: app-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

Mount in pod:

```yaml
volumes:
  - name: app-storage
    persistentVolumeClaim:
      claimName: app-pvc
```

### 🔹 Ingress Example

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: myapp-ingress
spec:
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: myapp-service
            port:
              number: 3000
```

---

## 🧩 6. Real-World DevOps Kubernetes Example

**Scenario:** Deploy multi-container Node.js + Nginx app

1. Create **Deployment** for backend (Node.js) and frontend (Nginx)
2. Create **Services** for both apps
3. Expose frontend via **Ingress**
4. Use **ConfigMaps** for environment variables
5. Scale pods as required for traffic
6. CI/CD pipeline triggers rolling updates via Jenkins / GitHub Actions

---

## 🏁 7. Hands-on Kubernetes Lab

**Steps:**

1. Minikube cluster (single-node for practice)

```bash
minikube start
```

2. Deploy backend:

```bash
kubectl apply -f backend-deployment.yaml
kubectl apply -f backend-service.yaml
```

3. Deploy frontend + ingress:

```bash
kubectl apply -f frontend-deployment.yaml
kubectl apply -f frontend-service.yaml
kubectl apply -f ingress.yaml
```

4. Test access:

```bash
curl http://myapp.example.com
```

5. Scale backend:

```bash
kubectl scale deployment backend-deployment --replicas=4
```

✅ Full end-to-end container orchestration with **self-healing, scaling, and external access**.

---

## 🏁 Summary

| Level           | Topics Covered                                                             |
| --------------- | -------------------------------------------------------------------------- |
| 🟢 Beginner     | Pods, Deployments, Services, basic kubectl commands                        |
| 🟡 Intermediate | ConfigMaps, Secrets, Persistent Storage, NodePort                          |
| 🔴 Advanced     | Rolling updates, scaling, Ingress, CI/CD integration, multi-container apps |

> 💬 “Kubernetes automates the management of containers, turning DevOps workflows into **scalable, reliable, and self-healing pipelines**.”

---


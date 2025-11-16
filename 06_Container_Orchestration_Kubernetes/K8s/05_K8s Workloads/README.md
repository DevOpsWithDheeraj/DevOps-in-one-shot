

# 🚀 **Kubernetes Workloads**

Kubernetes Workloads define **how your Pods should run**:
how many, on which nodes, how to restart them, how they scale, and how they persist.

Workload controllers:

1️⃣ ReplicaSets
2️⃣ StatefulSets
3️⃣ DaemonSets
4️⃣ Jobs
5️⃣ CronJobs

---

# 1️⃣ **ReplicaSet – Ensures a Fixed Number of Identical Pods**

A **ReplicaSet (RS)** ensures that **N replicas of a Pod are always running**.

### ✔ Key Features

* Maintains a **desired count** (e.g., always 3 Pods)
* Auto-recreate Pods when they crash
* Stateless workloads
* Rarely used directly → **Deployment uses ReplicaSet internally**

### 📌 When to use?

* Stateless microservices
* Nginx, frontend apps, APIs

---

### 📘 Example ReplicaSet

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: nginx
        image: nginx:latest
```

### 📊 Behavior

```
Desired Pods: 3

If 1 fails → RS creates a new one
If 1 extra appears → RS deletes it
```

---

# 2️⃣ **StatefulSet – For Stateful Applications**

A **StatefulSet (STS)** manages Pods that need:

* **Stable hostname**
* **Stable network identity**
* **Stable, persistent storage**
* **Ordered startup/shutdown**

### ✔ Key Features

* Pods names:

  ```
  web-0  
  web-1  
  web-2
  ```
* Each Pod gets **its own PersistentVolumeClaim**
* Used for databases & distributed systems:

  * MySQL
  * MongoDB
  * Kafka
  * Zookeeper
  * ElasticSearch

---

### 📘 Example StatefulSet

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: "mysql"
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:5.7
        volumeMounts:
        - name: mysql-data
          mountPath: /var/lib/mysql
  volumeClaimTemplates:
  - metadata:
      name: mysql-data
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 5Gi
```

### 📊 Behavior

```
Pod names: mysql-0, mysql-1, mysql-2
Each gets its own dedicated PVC:
  pvc-mysql-data-mysql-0
  pvc-mysql-data-mysql-1
  pvc-mysql-data-mysql-2
```

---

# 3️⃣ **DaemonSet – Runs One Pod on Every Node**

A **DaemonSet** ensures that **every node** in the cluster runs a copy of a specific Pod.

### ✔ Key Features

* Runs **one Pod per node**
* Automatically adds Pods when new nodes join
* Used for cluster-wide agents

### ✨ Common Use Cases

* Logging agents (Fluentd, Filebeat)
* Monitoring agents (Prometheus Node Exporter)
* Networking agents (Calico, Weave, Cilium)
* Storage agents

---

### 📘 Example DaemonSet

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: node-exporter
spec:
  selector:
    matchLabels:
      app: node-exporter
  template:
    metadata:
      labels:
        app: node-exporter
    spec:
      containers:
      - name: exporter
        image: prom/node-exporter
```

### 📊 Behavior

```
# Nodes: 3
node-exporter → runs on node1, node2, node3
```

---

# 4️⃣ **Job – Run a Task Once and Exit**

A **Job** runs Pods that:

* Run a task
* Finish successfully
* Do NOT restart forever

### ✔ Key Features

* Runs **batch jobs**
* Retries if a Pod fails
* Stops after successful completion

---

### ✨ Example Use Cases

* Database migrations
* Data backup
* File processing
* Sending emails
* Cleanup tasks

---

### 📘 Example Job

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: backup-job
spec:
  template:
    spec:
      restartPolicy: Never
      containers:
      - name: backup
        image: busybox
        command: ["sh", "-c", "echo taking backup... && sleep 5"]
```

### 📊 Behavior

```
Runs once → completes → stops.
If fails → new Pod is created.
```

---

# 5️⃣ **CronJob – Scheduled Jobs (like Linux cron)**

A **CronJob** runs Jobs on a **schedule**.

### ✔ Key Features

* Cron syntax: `"* * * * *"`
* Creates a **Job** at each scheduled time
* Retry if failure occurs
* Used for repeated batch tasks

### ✨ Use Cases

* Daily database backup
* Hourly cache cleanup
* Weekly reports
* Periodic notifications

---

### 📘 Example CronJob

Runs every minute:

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello-cron
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: Never
          containers:
          - name: hello
            image: busybox
            command: ["sh", "-c", "echo Hello from CronJob"]
```

### 📊 Behavior

```
Every 1 minute:
   CronJob → creates Job → Job runs Pod → Pod completes
```

---

# 🧠 Summary Table (Easy to Memorize)

| Workload        | Purpose                                | Pod Count                 | Use Case            |
| --------------- | -------------------------------------- | ------------------------- | ------------------- |
| **ReplicaSet**  | Ensures fixed number of identical Pods | Multiple                  | Stateless apps      |
| **StatefulSet** | Pods with identity & storage           | Multiple ordered          | DBs, Kafka, Redis   |
| **DaemonSet**   | One Pod on every node                  | One per node              | Logging, monitoring |
| **Job**         | Run once & exit                        | Multiple until successful | Batch processing    |
| **CronJob**     | Run Jobs on schedule                   | Multiple over time        | Backups, cleanup    |

---

# 🧠 Quick Visual

```
ReplicaSet → Many identical Pods (stateless)
StatefulSet → Ordered Pods with persistent storage
DaemonSet   → One Pod per Node
Job         → Run task once (batch)
CronJob     → Schedule Jobs (recurring)
```

---

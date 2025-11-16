

# 🚀 **Kubernetes Storage**

Kubernetes storage is based on one core idea:

> **Pods are ephemeral, but data must not be.**

When Pods die, restart, or reschedule to new Nodes → their **local storage is wiped**.
So Kubernetes introduces multiple storage concepts to **persist and manage data** reliably.

---

# 1️⃣ **Volume – Basic Storage attached to a Pod**

A **Volume** is the most basic storage abstraction.

### Key points:

* Lives as long as the **Pod** lives
* Not persistent across Pod recreation
* Examples: `emptyDir`, `hostPath`, `configMap`, `secret`, `nfs`, etc.

### 📘 Example: `emptyDir` (temporary storage)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: vol-example
spec:
  containers:
  - name: app
    image: nginx
    volumeMounts:
    - name: cache-storage
      mountPath: /cache
  volumes:
  - name: cache-storage
    emptyDir: {}
```

**Use-case:** Temporary cache directory.

---

# 2️⃣ **PersistentVolume (PV) – Actual Storage in the Cluster**

A **PV** represents **real storage** available in the cluster:

* Could be AWS EBS, GCP Disk, Azure Disk
* Or NFS, Ceph, GlusterFS
* **Created by admin or dynamically through StorageClass**

### Key features:

* Exists independently of Pods
* Cluster-wide resource
* Has capacity, access modes, reclaim policy

---

### 📘 Example PV (Static Provisioning)

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-nfs
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  nfs:
    path: /data/pv
    server: 10.0.0.5
```

---

# 3️⃣ **PersistentVolumeClaim (PVC) – Pod asks for Storage**

A **PVC** is how a Pod requests storage.

### Key points:

* Pods DO NOT directly use PVs
* A PVC **binds** to a matching PV
* Users specify:
  ✔ Size
  ✔ Access mode
  ✔ StorageClass (optional)

### 📘 Example PVC

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mypvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
```

K8s now searches for PVs with:

* ≥ 2Gi capacity
* ReadWriteOnce access mode
* Same StorageClass or no class

Then binds them.

---

### 🔗 Now Pod uses the PVC

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  containers:
  - name: app
    image: nginx
    volumeMounts:
    - mountPath: "/appdata"
      name: data
  volumes:
  - name: data
    persistentVolumeClaim:
      claimName: mypvc
```

💡 **Even if Pod recreates → data stays.**

---

# 4️⃣ **StorageClass (SC) – Automatic (Dynamic) Provisioning**

Without StorageClass = only **static provisioning**
(admin manually creates PVs)

With StorageClass = **automatic PV creation** when PVC is created.

### Key points:

* Defines *“how storage should be created”*
* Uses provisioners (AWS EBS, GCP PD, Ceph, etc.)
* PVC referencing SC triggers dynamic PV creation

---

### 📘 Example StorageClass (AWS EBS)

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: gp2
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
reclaimPolicy: Delete
volumeBindingMode: WaitForFirstConsumer
```

---

### 📘 PVC using this StorageClass

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: webapp-pvc
spec:
  storageClassName: gp2
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

Now Kubernetes automatically:

1. Creates PV
2. Binds PV ↔ PVC
3. Pod consumes PVC

🔥 No manual PV needed.

---

# 5️⃣ **CSI (Container Storage Interface)**

CSI is an industry-standard API that lets ANY storage provider integrate with Kubernetes.

### Before CSI:

* Kubernetes baked-in built storage (EBS, NFS, GCE PD)
* Hard to extend/support new vendors

### After CSI:

* Storage vendors ship plugins *outside Kubernetes*
* Kubernetes loads CSI driver as a Kubernetes component
* Very flexible & extensible

### Examples of CSI Drivers:

* AWS EBS CSI Driver
* GCP Persistent Disk CSI Driver
* Azure Disk CSI Driver
* Ceph RBD CSI
* OpenEBS
* Longhorn
* vSphere CSI

---

### 📘 How CSI Works in Practice

Flow:

```
PVC → StorageClass → CSI Driver → Creates PV → Mounted to Pod
```

This is the modern, recommended way for cloud-native storage.

---

# 🧠 Putting It All Together (Flow Diagram)

```
+-----------------+     PVC      +-----------------+
|     Pod         |  --------->  |  PVC (2Gi RWO)  |
|                 |  uses PVC    +-----------------+
+-----------------+                   |
                         finds/binds  |
                                       ↓
                               +---------------+
                               | PV (2Gi NFS) |
                               +---------------+
                                       |
                                 created by
                                       |
                               +---------------+
                               | StorageClass  |
                               +---------------+
                                       |
                                 provisioner
                                       ↓
                               +---------------+
                               |  CSI Driver   |
                               +---------------+
                                   (AWS EBS / NFS / Ceph / GCP)
```

---

# 🧠 Quick Summary Table

| Component        | Purpose                                        | Example            |
| ---------------- | ---------------------------------------------- | ------------------ |
| **Volume**       | Basic storage inside Pod                       | emptyDir, hostPath |
| **PV**           | Actual storage resource                        | 5Gi NFS storage    |
| **PVC**          | User request for storage                       | app needs 2Gi      |
| **StorageClass** | Dynamic provisioning                           | AWS EBS gp2 class  |
| **CSI**          | Plugin to integrate external storage providers | AWS EBS CSI driver |

---

# 💡 Real-life Analogy (Easy to Remember)

| K8s Component | Real-life Analogy                            |
| ------------- | -------------------------------------------- |
| Pod           | A person using a laptop                      |
| Volume        | Temporary RAM/SSD inside laptop              |
| PV            | A removable hard drive                       |
| PVC           | You request a specific hard drive            |
| StorageClass  | Rules for automatically buying hard drive    |
| CSI           | Amazon/Flipkart that supplies the hard drive |

---

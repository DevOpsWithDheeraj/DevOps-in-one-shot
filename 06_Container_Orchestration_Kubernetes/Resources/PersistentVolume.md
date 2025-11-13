
## 🧱 **What is PersistentVolume (PV)?**

A **PersistentVolume (PV)** is a **piece of storage in the cluster** that has been **provisioned by an administrator** or dynamically via **StorageClass**.

* PV exists independently of Pods.
* It provides a **persistent storage resource** that Pods can use.

> Think of a PV as a **storage resource in the cluster**, like a hard disk.

---

## 🧱 **What is PersistentVolumeClaim (PVC)?**

A **PersistentVolumeClaim (PVC)** is a **request for storage by a Pod**.

* Pod doesn’t request storage directly; it requests it via a PVC.
* PVC specifies **size**, **access mode**, and optionally **StorageClass**.
* Kubernetes **binds the PVC to a matching PV** automatically.

> Think of a PVC as a **Pod’s request for a hard disk**.

---

## 🔄 **PV & PVC Relationship**

```
Pod → PVC → PV → Storage
```

* Pod uses **PVC** to claim storage.
* PVC binds to **available PV** that matches the request.
* Pod can now **read/write persistent data** to the PV.

---

## 🧩 **PersistentVolume Example**

### YAML File: `pv.yaml`

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-demo
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/data  # Only for testing; in cloud use StorageClass
```

### Apply PV:

```bash
kubectl apply -f pv.yaml
```

### Check PV:

```bash
kubectl get pv
```

Output:

```
NAME      CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS
pv-demo   1Gi        RWO            Retain           Available
```

---

## 🧩 **PersistentVolumeClaim Example**

### YAML File: `pvc.yaml`

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-demo
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
```

### Apply PVC:

```bash
kubectl apply -f pvc.yaml
```

### Check PVC:

```bash
kubectl get pvc
```

Output:

```
NAME       STATUS    VOLUME     CAPACITY   ACCESS MODES
pvc-demo   Bound     pv-demo    1Gi        RWO
```

✅ PVC has **bound to PV** successfully.

---

## 🧩 **Using PVC in a Pod**

### YAML File: `pod-pvc.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-pvc-demo
spec:
  containers:
    - name: app
      image: nginx
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: storage
  volumes:
    - name: storage
      persistentVolumeClaim:
        claimName: pvc-demo
```

### Apply Pod:

```bash
kubectl apply -f pod-pvc.yaml
```

### Verify:

```bash
kubectl get pods
kubectl describe pod pod-pvc-demo
```

Now, the Pod has **persistent storage** mounted at `/usr/share/nginx/html`.
Even if the Pod restarts, data persists on the PV.

---

## 🔄 **Access Modes**

| Access Mode             | Meaning                                 |
| ----------------------- | --------------------------------------- |
| **ReadWriteOnce (RWO)** | Mounted as read-write by **one node**   |
| **ReadOnlyMany (ROX)**  | Mounted as read-only by **many nodes**  |
| **ReadWriteMany (RWX)** | Mounted as read-write by **many nodes** |

---

## 🔧 **PV Reclaim Policy**

| Policy      | Description                                               |
| ----------- | --------------------------------------------------------- |
| **Retain**  | Keep data even after PVC deletion (manual cleanup needed) |
| **Recycle** | Basic scrub of data before reuse (deprecated)             |
| **Delete**  | Delete PV and underlying storage automatically            |

---

## 🔑 **Dynamic Provisioning**

* If you create a PVC **without a pre-existing PV** and specify a **StorageClass**, Kubernetes will **dynamically provision a PV**.
* Example StorageClass in cloud providers:

  * AWS → `gp2` EBS volume
  * GCP → `standard` Persistent Disk
  * Azure → `managed-premium`

```yaml
spec:
  storageClassName: standard
  resources:
    requests:
      storage: 1Gi
```

---

## 🔍 **Visual Overview**

```
Pod
 └─ uses PVC (request storage)
       └─ bound to PV (actual storage)
             └─ hostPath / cloud storage
```

---

## ✅ **Summary**

| Concept                         | Description                                          |
| ------------------------------- | ---------------------------------------------------- |
| **PersistentVolume (PV)**       | Storage resource in the cluster (hard disk)          |
| **PersistentVolumeClaim (PVC)** | Pod’s request for storage                            |
| **Binding**                     | PVC binds to matching PV                             |
| **Access Modes**                | RWO, ROX, RWX                                        |
| **Reclaim Policy**              | Retain, Recycle, Delete                              |
| **Dynamic Provisioning**        | Kubernetes creates PV automatically via StorageClass |

---

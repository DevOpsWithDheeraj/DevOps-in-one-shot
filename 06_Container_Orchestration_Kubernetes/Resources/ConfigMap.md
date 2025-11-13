
## ⚙️ **What is a ConfigMap in Kubernetes?**

A **ConfigMap** is a Kubernetes object that **stores configuration data** as **key-value pairs**.

* Pods can use ConfigMaps to get **environment variables, command-line arguments, or configuration files**.
* This allows **decoupling configuration from container images**, so you don’t have to rebuild images to change configuration.

---

### 🧠 **Why Do We Need ConfigMaps?**

* Avoid **hardcoding configuration** inside containers.
* Manage **different environment configurations** (dev, staging, prod).
* Update configuration **without restarting the container** in some cases.

---

## 🧩 **How ConfigMap Works**

```
ConfigMap (key-value pairs)
      │
      ▼
Pod consumes via:
- Environment Variables
- Volume Mounts (as files)
- Command Arguments
```

---

## 📄 **Example 1: ConfigMap from Literal Values**

### YAML File: `configmap-demo.yaml`

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: production
  APP_LOG_LEVEL: info
```

### Apply ConfigMap:

```bash
kubectl apply -f configmap-demo.yaml
```

### Verify:

```bash
kubectl get configmap
kubectl describe configmap app-config
```

---

## 📄 **Example 2: Using ConfigMap as Environment Variables in a Pod**

### Pod YAML: `pod-configmap.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-configmap-demo
spec:
  containers:
    - name: app
      image: nginx
      envFrom:
        - configMapRef:
            name: app-config
```

### Explanation:

* `envFrom` → injects all key-value pairs from the ConfigMap as **environment variables**.
* Inside the Pod, you can check:

```bash
kubectl exec -it pod-configmap-demo -- env
```

You will see:

```
APP_ENV=production
APP_LOG_LEVEL=info
```

---

## 📄 **Example 3: Using ConfigMap as a File (Volume Mount)**

### Pod YAML: `pod-configmap-volume.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-configmap-volume
spec:
  containers:
    - name: app
      image: nginx
      volumeMounts:
        - name: config-volume
          mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: app-config
```

* Each key in the ConfigMap becomes a **file inside `/etc/config`**.
* File content = key’s value.
  Example:

```
/etc/config/APP_ENV        → production
/etc/config/APP_LOG_LEVEL  → info
```

---

## 🔧 **Common Commands**

| Command                                                    | Description                        |
| ---------------------------------------------------------- | ---------------------------------- |
| `kubectl get configmap`                                    | List ConfigMaps                    |
| `kubectl describe configmap <name>`                        | Show ConfigMap details             |
| `kubectl create configmap <name> --from-literal=key=value` | Create ConfigMap from command line |
| `kubectl create configmap <name> --from-file=<path>`       | Create from file                   |
| `kubectl delete configmap <name>`                          | Delete ConfigMap                   |

---

## 🧠 **Best Practices**

* Do **not store sensitive data** in ConfigMaps (use Secrets instead).
* Use ConfigMaps for **non-sensitive configuration** like environment flags, logging levels, or app URLs.
* Version your ConfigMaps if needed to manage rolling updates.

---

## 🔍 **Visual Overview**

```
ConfigMap: app-config
 ├─ APP_ENV=production
 └─ APP_LOG_LEVEL=info

Pod
 ├─ Uses as Environment Variables → APP_ENV, APP_LOG_LEVEL
 └─ Or mounted as Volume → /etc/config/APP_ENV, /etc/config/APP_LOG_LEVEL
```

---

✅ **Summary**

| Concept            | Description                                                |
| ------------------ | ---------------------------------------------------------- |
| **ConfigMap**      | Stores non-sensitive configuration data as key-value pairs |
| **Consumption**    | Environment variables, command arguments, volume files     |
| **Benefits**       | Decouple config from image, easier updates                 |
| **Do NOT use for** | Secrets / sensitive data (use Secret object instead)       |

---

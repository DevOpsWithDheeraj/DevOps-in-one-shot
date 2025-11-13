
## ⚙️ **What is a Secret in Kubernetes?**

A **Secret** is a Kubernetes object used to **store sensitive information** in the cluster:

* Passwords
* API keys
* Tokens
* TLS certificates

Secrets allow Pods to access this data **without hardcoding it in the container** or ConfigMap.

---

### 🧠 **Why Do We Need Secrets?**

* **Security**: Don’t store passwords or keys in plain text inside Pods or images.
* **Decoupling**: Keep secrets separate from application code.
* **Dynamic Updates**: Secrets can be updated without rebuilding images.

> ⚠️ Kubernetes **base64-encodes** Secrets by default — not encrypted at rest unless you enable encryption.

---

## 🔄 **How Secrets Work**

```
Secret (key-value, base64 encoded)
      │
      ▼
Pod consumes via:
- Environment Variables
- Volume Mounts (as files)
- ImagePullSecrets (for private registries)
```

---

## 📄 **Example 1: Creating a Secret from Literal Values**

### YAML File: `secret-demo.yaml`

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
data:
  username: YWRtaW4=       # base64 for "admin"
  password: MWYyZDFlMmU2N2Rm # base64 for "1f2d1e2e67df"
```

### Apply Secret:

```bash
kubectl apply -f secret-demo.yaml
```

### Verify:

```bash
kubectl get secrets
kubectl describe secret app-secret
```

To decode:

```bash
echo "YWRtaW4=" | base64 --decode   # outputs: admin
```

---

## 📄 **Example 2: Creating a Secret from Command Line**

```bash
kubectl create secret generic app-secret-cli \
  --from-literal=username=admin \
  --from-literal=password=1f2d1e2e67df
```

---

## 📄 **Example 3: Using Secret as Environment Variables in a Pod**

### Pod YAML: `pod-secret-env.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-secret-demo
spec:
  containers:
    - name: app
      image: nginx
      env:
        - name: APP_USERNAME
          valueFrom:
            secretKeyRef:
              name: app-secret
              key: username
        - name: APP_PASSWORD
          valueFrom:
            secretKeyRef:
              name: app-secret
              key: password
```

* Inside the Pod, `APP_USERNAME` and `APP_PASSWORD` environment variables hold the secret values.

---

## 📄 **Example 4: Using Secret as a Volume**

### Pod YAML: `pod-secret-volume.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-secret-volume
spec:
  containers:
    - name: app
      image: nginx
      volumeMounts:
        - name: secret-volume
          mountPath: "/etc/secret"
          readOnly: true
  volumes:
    - name: secret-volume
      secret:
        secretName: app-secret
```

* Keys in the Secret become **files** inside `/etc/secret`.
* For example:

```
/etc/secret/username → "admin"
/etc/secret/password → "1f2d1e2e67df"
```

---

## 🔧 **Secret Types**

| Type                                    | Description                  |
| --------------------------------------- | ---------------------------- |
| **Opaque**                              | Generic secret (default)     |
| **kubernetes.io/dockerconfigjson**      | Docker registry credentials  |
| **kubernetes.io/tls**                   | TLS certificate (cert + key) |
| **kubernetes.io/service-account-token** | Token for service accounts   |

---

## 🔑 **Best Practices**

* **Do not commit Secrets to Git**.
* **Use RBAC** to restrict Secret access.
* **Enable encryption at rest** in production clusters.
* Use **Secrets with StatefulSets or Deployments** for secure storage.

---

## 🔍 **Visual Overview**

```
Secret: app-secret
 ├─ username: admin
 └─ password: 1f2d1e2e67df

Pod
 ├─ Access via Env Variables → APP_USERNAME, APP_PASSWORD
 └─ Access via Volume → /etc/secret/username, /etc/secret/password
```

---

## ✅ **Summary**

| Concept           | Description                                              |
| ----------------- | -------------------------------------------------------- |
| **Secret**        | Stores sensitive data (passwords, tokens, certificates)  |
| **Consumption**   | Environment variables, volume files, image pull secrets  |
| **Type**          | Opaque, TLS, Docker config, ServiceAccount token         |
| **Security**      | Base64 encoded; enable encryption at rest for production |
| **Best Practice** | Never hardcode secrets in images; use RBAC               |

---
